# CD/CI chain

**Table of content**.

* [Link](#link)
* [Description](#description)
* [Setup and configuration](#setup-and-configuration)

**Team Roles**

Anders Bjergfelt: Dev Ops, Project Manager. 

Daniel Hillmann: Testing Lead (Q&A)

Emil Gräs: Architect

Frederik Bothmann Larsen: Lead Developer

---
## Link

[Link to backend-application](http://46.101.190.192:8080/status)

[Frontend Project](https://github.com/gode-ting/hackerNews-clone-project-frontend)

[Backend Project](https://github.com/gode-ting/hackernews-clone-backend)

Project setup:

VCS: Git and GitHub as host.

 
Build Server: Travis CI


Docker containers and DockerHub as public registry.


Vagrant, creation and handling virtual machines.


Cloud server provider – Digital Ocean.

## Setup and configuration

### Build setup: Travis CI

Travis CI is a hosted, distributed continuous integration service used to build and test software projects hosted at GitHub.

Travis CI is configured by adding a file named .travis.yml, which is a YAML format text file, to the root directory of the repository.

When Travis CI has been activated for a given repository, GitHub will notify it whenever new commits are pushed to that repository or a pull request is submitted. 

It can also be configured to only run for specific branches. 

*In our case* it will only run when pushed or merged into **test branch**

```yaml
branches:
  only:
  - test
```

## Walkthrough 

*Build stages:*

**What are Build Stages?**

*Build stages is a way to group jobs, and run jobs in each stage in parallel, but run one stage after another sequentially.*

**However**, each one of the stages runs one after another, and will only proceed if all jobs in the previous stage have passed successfully. If one job fails in one stage, all other jobs on the same stage will still complete, 
but all jobs in subsequent stages will be canceled, and the build fails.

Here’s how we have set up the build configuration in our .travis.yml file:

```yaml
before_install:
install:
after_success:
```
## Before install

```yaml
before_install:
- openssl aes-256-cbc -K $encrypted_2b560cfeafab_key -iv $encrypted_2b560cfeafab_iv -in travis_rsa.enc -out travis_rsa -d
- chmod +x mvnw
- rm travis_rsa.enc
- chmod 600 travis_rsa
- mv travis_rsa  ~/.ssh/id_rsa
```
Because we want Travis to connect to our server hosted at Digital Ocean we'll need to provide Travis with a key. 
We use [The Travis Client](https://github.com/travis-ci/travis.rb) to encrypt our public key.

Afterwards it will create a *travis_rsa.enc* file and decrypt it again:

```yaml
- openssl aes-256-cbc -K $encrypted_2b560cfeafab_key -iv $encrypted_2b560cfeafab_iv -in travis_rsa.enc -out travis_rsa -d
```

## Install
```yaml
install:
- mvn -N io.takari:maven:wrapper
- "./mvnw install -DskipTests=true -Dmaven.javadoc.skip=true -B -V"
```
The Maven Wrapper is an easy way to ensure a user of your Maven build has everything necessary to run your Maven build.

**Why might this be necessary?** Maven to date has been very stable for users, is available on most systems or is easy to procure: but with many of the recent changes in Maven it will be easier for users to have a fully encapsulated build setup provided by the project. 

The easiest way to setup the Maven Wrapper for our project is to use the Takari Maven Plugin with its provided wrapper goal. 

To add or update all the necessary Maven Wrapper files we execute the following command:

```yaml
- mvn -N io.takari:maven:wrapper
```
## After success

```yaml
after_success:
- docker build -t abjdocker/hackernewscloneserver:101 .
- set +x
- if [ "$TRAVIS_BRANCH" == "test" ]; then docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD";
  docker push abjdocker/hackernewscloneserver:101;
  docker logout;
  fi
- ssh builder@46.101.190.192 "./deploy2.sh 101"
```
We build a docker image based on this Dockerfile:

```yaml
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD  ./target/hackernews-clone-backend-0.0.1-SNAPSHOT.jar app.jar
ENV JAVA_OPTS=""
ENTRYPOINT exec java $JAVA_OPTS -Dspring.profiles.active=production -Djava.security.egd=file:/dev/./urandom -jar /app.jar
```

If succesful it'll push the image to Dockerhub.

Then it'll log onto our server and run deploy2.sh:

```bash
#!/bin/bash
BUILD_NUMBER=$1
DOCKER_ID=$2
# stop all running containers with our web application
docker stop `docker ps -a | grep abjdocker/hackernewscloneserver | awk '{print substr ($0, 0, 12)}'` .
# remove all of those containers
docker rm `docker ps -a | grep abjdocker/hackernewscloneserver | awk '{print substr ($0, 0, 12)}'` .
# TODO remove old image
# get the newest version of the containerized web application and run it
docker pull abjdocker/hackernewscloneserver:${BUILD_NUMBER}
docker run -d -ti -p 8080:8080 abjdocker/hackernewscloneserver:${BUILD_NUMBER}
```





## NOTES:

## Docker containers

A Docker Image acts as a blueprint or template for a Docker Container. For example *docker run openjdk:8-jdk-alpine* will create a new Docker Container based on the *openjdk:8-jdk-alpine* Docker Image.

The preferred way to create a Docker Image is with a script known as a Dockerfile. Alternatively the required changes can be made through running a shell on a container and then committing it to an image. Dockerfiles have the advantage of providing the ability to automate image creation and the syntax is simple, clear and self explanatory.

To build a Docker Image from our Spring Application is to add the Dockerfile that contains the instructions for Docker to build the Image for the application.

We also use a mongodb container on prod server. 

It can be accessed, when logged into the server:
*docker exec -it [container name/ID] sh*


```shell
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD  ./target/hackernews-clone-backend-0.0.1-SNAPSHOT.jar app.jar
ENV JAVA_OPTS=""
ENTRYPOINT exec java $JAVA_OPTS -Dspring.profiles.active=production -Djava.security.egd=file:/dev/./urandom -jar /app.jar
```
## Vagrant - virtual machines

Vagrant is a tool for building and managing virtual machine environments in a single workflow. With an easy-to-use workflow and focus on automation, Vagrant lowers development environment setup time, increases production parity, and makes the "works on my machine" excuse a relic of the past.

Right now the only VM we are using is our local db.

```shell
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  #config.vm.network "forwarded_port", guest: 80, host: 8080
  #config.vm.network "forwarded_port", guest: 8080, host: 8080
  # mongodb port
  #config.vm.network "forwarded_port", guest: 27017, host: 27017

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "forwarded_port", guest: 8080, host: 8081
  
  # for mongoDB server
  config.vm.network "forwarded_port", guest: 27017, host: 27018
  # config.vm.network "forwarded_port", guest: 27018, host: 27018
  # config.vm.network "forwarded_port", guest: 27019, host: 27019
  config.vm.network "forwarded_port", guest: 28017, host: 28017
  # for MySQL DB server
  config.vm.network "forwarded_port", guest: 3306, host: 3307
  # config.vm.network "private_network", type: "dhcp"
	config.vm.network "private_network", ip: "192.168.30.20"
  # config.vm.network "public_network"
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.name = "dbprod"
  end
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "Installing MongoDB"
    sudo apt-get -y install mongodb-server
    sudo mkdir -p /data/db
    # TODO:
    # /etc/mongodb.conf modify bind_ip line to 0.0.0.0 and restart server
    sudo sed -i '/bind_ip = / s/127.0.0.1/0.0.0.0/' /etc/mongodb.conf
    # sudo systemctl restart mongod
    sudo service mongodb restart
    # sudo mongod
    # sudo service mysql restart
    # sudo /etc/init.d/mysql restart
  SHELL
end

```



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

*OR* I can send you a private key. Much easier.

Afterwards you can configure PuTTY to use your private key file.

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

# F.A.Q

**We have just deployed and application is down. How do we debug?**

1. Log into server
2. run *docker ps* if you cannot see it, it's probably down. 
3. run *docker logs {container_image_id}* {container_image_id} can be found at the end of every Travis build.

example:
```bash
10.80s$ ssh builder@46.101.190.192 "./deploy2.sh 101"
b16c05406756
.
b16c05406756
.
101: Pulling from [secure]/hackernewscloneserver
b56ae66c2937: Already exists
2296e775ba08: Already exists
6e753bb2ec67: Already exists
cebcfccbc79f: Pulling fs layer
cebcfccbc79f: Verifying Checksum
cebcfccbc79f: Download complete
cebcfccbc79f: Pull complete
Digest: sha256:6c0697f5e1a0d3373b386c96a3d472af4c4840df0e3f3589cb7f0a12664c4d91
Status: Downloaded newer image for [secure]/hackernewscloneserver:101
9a51c79eb17b1c117dbc5fdece94706596fbb90d8bde3666b6dea89a9ca16823
```
*9a51c79eb17b1c117dbc5fdece94706596fbb90d8bde3666b6dea89a9ca16823* is {container_image_id}






