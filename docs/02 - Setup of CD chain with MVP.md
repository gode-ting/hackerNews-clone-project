# Setup of CD chain with MVP

**Table of content**.

* [Link](#link)
* [Description](#description)
* [Setup and configuration](#setup-and-configuration)

---

## Link

[Link to the MVP application](http://46.101.190.192:8080/hello/world)

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




