# Setup of CD chain with MVP

**Table of content**.

* [Link](#link)
* [Description](#description)
* [Setup and configuration](#setup-and-configuration)

---

## Link

[Link to the MVP application](http://46.101.190.192:8080/hello/world)

[Frontend Project](https://github.com/andersbjergfelt/hackernews-clone-frontend)
[Backend Project](https://github.com/andersbjergfelt/hackernews-clone-backend)

## Description

Project setup:

VCS: Git and GitHub as host.

 
Build Server: Jenkins


Docker containers and DockerHub as public registry.


Vagrant, creation and handling virtual machines.


Maven repository manager – Artifactory.


Cloud server provider – Digital Ocean.


## Setup and configuration

### Build server setup: Jenkins

Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and deploying software.

Since, we do not have access to a proper build server, i.e., a separate machine, we decide to setup an Ubuntu virtual machine (VirtualBox), which will host our Jenkins build server. 

[Get Vagrant file and provision.sh from here](https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/tree/master/lecture_notes/05-Code_Infrastructure/vm)

Place it somewhere safe

cd to the directory with the Vagrantfile and startup the VM. When started up for the first time vagrant up will automatically run the provision script (provision.sh).

After starting the VM Jenkins should be up and running. You can access it via http://localhost:9090

## Configuring Jenkins

Now that Jenkins is running you have to configure it. On first time use it will present you the following page "Unlock Jenkins"

Here you have to insert the key that you get either from the output of the provision script or via:

```shell
vagrant ssh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Subsequently, choose to Install suggested plugins. we will install the other required plugins later manually.
Afterwards, create a first admin user on Jenkins. Call it builder.

Now, install some more plugins that we need to build our projects.
Navigate to Manage Jenkins -> Manage Plugins -> Available and select the following plugins
* Maven Integration plugin
* Artifactory Plugin

After selection, hit Download now and install after restart and on the next page check Restart Jenkins when installation is complete and no jobs are running and wait until Jenkins is restarted.

Now, we configure how Jenkins finds your Maven installation. For this project we will download Maven dynamically: Navigate to Manage Jenkins -> Global Tool Configuration and set the Maven section similar to the following and save.

//INSERT COOL PICTURE HERE 

Navigate to Manage Jenkins -> Configure System, set the Artifactory section to point to your Artifactory instance and test the connection to it. In case you did not modify the configuration of your Artifactory instance, then the standard login is admin and the standard password is password.

//INSERT COOL PICTURE HERE 

## Adding Credentials to Jenkins
To allow for the later use of DockerHub as registry for the container with the final web application we are registered at 
[https://hub.docker.com](https://hub.docker.com)

After you have created a user at DockerHub, navigate to Credentials -> (global) -> Add Credentials (which corresponds to navigating to the following URL: http://localhost:8080/credentials/store/system/domain/_/ ).

There add a Secret text, where the secret is your password to your DockerHub account. (message me for pwd)

//INSERT A COOL PICTURE HERE

## Allow Jenkins User to Execute Deployment Script Remotely

To allow for a login from a non-interactive shell to the remote machine we have to create a pair of SSH keys for the jenkins user, i.e., the Linux user executing the shell scripts in Freestyle jobs.

You enable non-interactive login to the remote machine by logging to the VM, switching to the jenkins user, creating a pass-phrase-less key pair and moving them to the remote machine.

```shell
vagrant ssh
sudo su jenkins
ssh-keygen -t rsa -b 2048
cat /var/lib/jenkins/.ssh/id_rsa.pub
```
Copy the output of the cat command, i.e., the jenkins users' public key into you clipboard. From another terminal session connect to your remote server at DigitalOcean and register yet another public key.

```shell
ssh builder@46.101.190.192
nano ~/.ssh/authorized_keys
```
*nano* is a small and friendly text editor. 

To verify that the remote login based on the keys is working, ssh to the machine from the jenkins user in your Vagrant VM:
ssh builder@<your_ip>
Afterwards, to exit from the remote machine, from the jenkins user, and from the VM type:
exit

## Build Jobs

In total we will create 3 build jobs. Two Maven build jobs and one Freestyle build job. The latter can incorporate anything and we will use it to execute shell commands to build and deploy our docker containers.

The build jobs are the following:

* hackernews-backend(Maven project build job)
* hackernews-clone-frontend (Maven project build job)
* hackernews-clone-build-docker(Freestyle project build job)

## A Maven Project Build Job for hackernews-backend

This job will, with the help of Maven, build the *hackernews-backend code*, create a JAR file from it and it will push the JAR file to Artifactory so that it can be resolved as dependency from other projects.

Navigate to New Item, select Maven project, and give it the name hackernews-backend. Subsequently, under Source Code Management choose Git and point it to corresponding repository on GitHub, see the following screenshot.

//INSERT COOL SCREENSHOT

This build job -as configured so far- will build a JAR file on your build machine. You can try that by hitting *Build Now*, which schedules the build job. In the list of the build jobs on the left side you can select the one you just triggered and hit Console Output to verify that the build job just created a JAR file for you.

Now, we have to make this JAR file accessible to others by registering it to Artifactory. You do that by adding a Add post-build action -> *Deploy artifacts to Artifactory*, in which you point to your Artifactory server and the corresponding release and snapshot repositories, see the following screenshot.


//INSERT COOL SCREENSHOT

Note, we did not setup any Build Triggers so far as for this example we schedule the build job manually. You can select Build Triggers to your project needs, such as Build when a change is pushed to GitHub or Build periodically.

## A Maven Project Build Job for hackernews-clone-frontend

This job will, with the help of the Gradle Wrapper gradlew (https://docs.gradle.org/current/userguide/gradle_wrapper.html), build the hackernews-clone-frontend code. The built code will be consumed by the subsequent freestyle Docker build job.

Navigate to New Item, select Maven project, and give it the name choir-backend-mock. Subsequently, under Source Code Management choose Git and point it to corresponding repository on GitHub, see the following screenshot.

//INSERT COOL SCREENSHOT

Now, we have to make this JAR file accessible to others by registering it to Artifactory. You do that by adding a Add post-build action -> Deploy artifacts to Artifactory, in which you point to your Artifactory server and the corresponding release and snapshot repositories, see the following screenshot.

//INSERT COOL SCREENSHOT

Note, we did not setup any Build Triggers we schedule the build job manually. You can select Build Triggers to your project needs, such as Build when a change is pushed to GitHub or Build periodically, or Build after other projects are built, e.g., after choir-contract is built.

Furthermore, when building your own code similar to this you have to remember to modify your pom.xml accordingly. There you have to add another repository to resolve dependencies like this:

```java

<dependency>
            <groupId>com.daef</groupId>
            <artifactId>hackernews-clone-backend</artifactId>
            <version>0.0.1-SNAPSHOT</version>
</dependency> 
<repositories>
      <repository>
        <id>hackernews-clone-repo</id>
        <name>Daef Repo</name>
        <url>http://46.101.190.192:8082/artifactory/libs-release-local</url>
      </repository>
      <repository>
        <id>hackernews-clone-repo-snapshot</id>
        <name>Daef Repo</name>
        <url>http://46.101.190.192:8082/artifactory/libs-snapshot-local</url>
      </repository>
    </repositories>
```
Now, under Build -> Add build step choose Execute shell. Paste the following shell script into the Command field.

```shell
./gradlew build
```

## A Freestyle Build Job for hackernews-clone-build-docker

This job deploys the JAR file ([Because we use SPRING FRAMEWORK](https://spring.io/blog/2014/03/07/deploying-spring-boot-applications)) -which was generated by the previous build step- to a Docker container, which in turn is registered at the DockerHub. From there the container is deployed automatically to your remote production machine.

That is, the build job consists of two build steps. One for building the Docker container and distributing it to the DockerHub and a second one for deploying that container to the remote machine. Both build steps execute a sequence of shell commands.

Navigate to New Item, select Freestyle project, and give it the hackernews-clone-build-docker. Subsequently, under Source Code Management choose Git and point it to the repository on GitHub containing the Dockerfile, see the following screenshot.

//INSERT COOL SCREENSHOT

Now, under Build -> Add build step choose Execute shell. Paste the following shell script into the Command field.

```shell
chmod --recursive a+rwx  /var/lib/jenkins/workspace/hackernews-clone-frontend ${WORKSPACE}
cp -r /var/lib/jenkins/workspace/hackernews-clone-frontend ${WORKSPACE}
docker build -t abjdocker/hackernewscloneserver:${BUILD_NUMBER} .

set +x
docker login -u abjdocker -p "${DOCKERHUB_PWD}"
set -x

docker push abjdocker/hackernewscloneserver:${BUILD_NUMBER}
docker logout
```
*chmod --recursive a+rwx* recursively change file permission
 
The *set +x* suppresses echoing the operation to the console output. Your password should not appear there. With respect to Jenkins your password to the DockerHub is stored in the variable ${DOCKERHUB_PWD}. To fill this variable accordingly you have to add a secret text to the configuration of the credentials plugin, see above. Furthermore, you have to configure the use of this as a Secret text under Build Environment as illustrated in the following screenshot.

//INSERT COOL SCREENSHOT

Now, the newly build Docker image, which is registered to the DockerHub has to be deployed to the remote machine. For this, add another build step. Under Build -> Add build step choose Execute shell and paste the following shell script into the Command field.

```shell
ssh builder@46.101.190.192 "./deploy2.sh ${BUILD_NUMBER} abjdocker"
```
This will call the remote deployment script, which was put there during machine setup.

```shell
#!/bin/bash

BUILD_NUMBER=$1
DOCKER_ID=$2
# stop all running containers with our web application
docker stop `docker ps -a | grep ${DOCKER_ID}/hackernewscloneserver | awk '{print substr ($0, 0, 12)}'`
# remove all of those containers
docker rm `docker ps -a | grep ${DOCKER_ID}/hackernewscloneserver | awk '{print substr ($0, 0, 12)}'`
# TODO remove old image
# get the newest version of the containerized web application and run it
docker pull ${DOCKER_ID}/hackernewscloneserver:${BUILD_NUMBER}
docker run -d -ti -p 8080:8080 ${DOCKER_ID}/hackernewscloneserver:${BUILD_NUMBER}
```
## DONE WITH BUILD JOBS

## Docker containers

A Docker Image acts as a blueprint or template for a Docker Container. For example *docker run openjdk:8-jdk-alpine* will create a new Docker Container based on the *openjdk:8-jdk-alpine* Docker Image.

The preferred way to create a Docker Image is with a script known as a Dockerfile. Alternatively the required changes can be made through running a shell on a container and then committing it to an image. Dockerfiles have the advantage of providing the ability to automate image creation and the syntax is simple, clear and self explanatory.

To build a Docker Image out of our Spring Application is to add the Dockerfile that contains the instructions for Docker to build the Image for the application.

```shell
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD ./target/hackernews-clone-frontend-0.0.1-SNAPSHOT.jar app.jar
ENV JAVA_OPTS=""
ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar
```

## Vagrant - virtual machines

Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the "works on my machine" excuse a relic of the past.

Right now the only VM we are using is our "build server"

```shell
Vagrant.configure(2) do |config|

  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "2048"
    vb.name = "cph-buildsrv"
  end
  # config.vm.provision "shell", inline: $script
  config.vm.provision "shell", path: "provision.sh"

  config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.network "forwarded_port", guest: 8080, host: 9090
  # enable this to share remote access with others
  config.vm.network "public_network"
end
```
*provision.sh* installs Jenkins. 

**Maven repository manager – Artifactory.(OPEN SOURCE)

*A repository manager is a dedicated server application designed to manage repositories of binary components. The usage of a repository manager is considered an essential best practice for any significant usage of Maven*

A repository manager serves these essential purposes:

* act as dedicated proxy server for public Maven repositories
* provide repositories as a deployment destination for your Maven project outputs

### Benefits and Features

Using a repository manager provides the following benefits and features:

* significantly reduced number of downloads off remote repositories, saving time and bandwidth resulting in increased build performance

* improved build stability due to reduced reliance on external repositories

* increased performance for interaction with remote SNAPSHOT repositories

* potential for control of consumed and provided artifacts

* creates a central storage and access to artifacts and meta data about them exposing build outputs to consumer such as other projects and developers, but also QA or operations teams or even customers

* provides an effective platform for exchanging binary artifacts within your organization and beyond without the need for building artifact from source

Our remote machine (PROD) is running with an Artifactory instance.
It is done in the initial server setup.

*FROM setup.sh*
```shell
  artifactory:
    image: mattgruter/artifactory:3.9.2
    hostname: artifactory
    ## build: artifactory/
    ports:
      - "8082:8080"
    volumes:
      - /etc/localtime:/etc/localtime
      - /artifactory/data
      - /artifactory/logs
      - /artifactory/backup
    environment:
      - JAVA_OPTS='-Djsse.enableSNIExtension=false'
```
To validate: point your browser to http://<your_ip>/:8082 where you should see documentation on how to connect to the Artifactory instance.

## Cloud server provider – Digital Ocean.

There are many different cloud computing providers, such as Amazon AWS, Google Cloud Platform, IBM Bluemix, etc.

We use *Digital Ocean*

To log onto your remote servers more easily and properly you setup a pair of SSH keys and register the public key with DigitalOcean. 

### How to log in on Windows

use PuTTY, an SSH client. 

Once PuTTY is installed, start the program.

On the PuTTY Configuration screen that opens, fill in the Host Name (or IP address) field with the Droplet's IP address. Confirm that the Port is set to 22 and that the Connection type SSH is selected.

Once everything is configured, you can save these settings for future logins by entering a title into the Saved Sessions field. Click Save to store settings.

Before you connect to a server for the first time, PuTTY will ask you to confirm that you trust the server. Choose Yes to save the server identity in PuTTY's cache or No to connect without saving the identity.

After PuTTY starts, type in the root password that was emailed to you. Note that if you uploaded keys, you will either be connected directly or prompted for the password you set on your key.


*Remember to use PuTTYgen to generate SSH keys.*

PuTTYgen is an key generator tool for creating SSH keys for PuTTY. It is analogous to the ssh-keygen tool used in some other SSH implementations.

The basic function is to create public and private key pairs. PuTTY stores keys in its own format in .ppk files. However, the tool can also convert keys to and from other formats.

To create a new key pair, select the type of key to generate from the bottom of the screen (using SSH-2 RSA with 2048 bit key size is good for most people; another good well-known alternative is ECDSA).

Then click Generate, and start moving the mouse within the Window. Putty uses mouse movements to collect randomness.

You should save at least the private key by clicking Save private key. It may be advisable to also save the public key, though it can be later regenerated by loading the private key (by clicking Load).

### INSTALLING THE PUBLIC KEY AS AN AUTHORIZED KEY ON A SERVER

Send your public key to me and I will add it to *~/.ssh/authorized_keys* 
Afterwards you can configure PuTTY to use your private key file.

//INSERT COOL SCREENSHOT

### Manually generating your SSH key in Mac OS X

```shell
ssh-keygen -t rsa
```
This starts the key generation process. When you execute this command, the ssh-keygen utility prompts you to indicate where to store the key.

Press the ENTER key to accept the default location. The ssh-keygen utility prompts you for a passphrase.

Type in a passphrase. You can also hit the ENTER key to accept the default (no passphrase). However, this is not recommended.
Warning! You will need to enter the passphrase a second time to continue.

After you confirm the passphrase, the system generates the key pair.

*Your private key is saved to the id_rsa file in the .ssh directory*

*Never share your private key with anyone!*

Your public key is saved to the id_rsa.pub;file 

NOTE: send public key and I'll add it to *~/.ssh/authorized_keys* 

Afterwards you can use your private key to access our remote server on Digital Ocean.










