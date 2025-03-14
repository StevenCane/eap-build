[![Build Status](https://travis-ci.com/hasalex/eap-build.svg?branch=master)](https://travis-ci.com/hasalex/eap-build)

Building JBoss EAP, or something similar...

Why ?
=====
As I was not able to build JBoss EAP 6+, I've made a script who can download JBoss EAP 6+'s source code, patch the repository and launch the build with a JBoss Maven repository.

The result isn't exactly a JBoss EAP binary but something with a few differences.

How ?
=====
You can get the build script with git or wget.

With git
--------
If you want to run the script :

    git clone git://github.com/hasalex/eap-build.git
    cd eap-build
    ./build-eap7.sh

For EAP 8 versions, you should use

    ./build-eap8.sh

For EAP 7 versions, you should use

    ./build-eap7.sh

By default, it builds the latest EAP 7 update. You can build other versions by passing the number to the build :

    ./build-eap7.sh 7.2.3

For EAP 6 versions, you should use 

    ./build-eap6.sh

By default, it builds the latest EAP 6 update. You can build other versions by passing the number to the build :

    ./build-eap6.sh 6.4.19

Without git
-----------
If you don't want to use git, download the archive, unzip it and run the main script :

    wget https://github.com/hasalex/eap-build/archive/master.zip
    unzip master.zip
    cd eap-build-master
    ./build-eap7.sh

Versions
--------
The build-eap8.sh script supports 8.0.0, 8.0.4, 8.0.6

The build-eap7.sh script supports 7.0.0->7.0.9, 7.1.0->7.1.4, 7.2.0->7.2.9, 7.3.0->7.3.10, 7.4.0->7.4.21

The build-eap6.sh script supports 6.1.1, 6.2.0->6.2.4, 6.3.0->6.3.3, 6.4.0->6.4.23

Docker build
============

You may build a docker image :

    docker build --tag hasalex/eap-build --file docker/Dockerfile-debian .

And run it :

    docker run --interactive --tty --publish 8080:8080 --publish 9990:9990 hasalex/eap-build

With a deployment, in detached mode :

    docker run --detach --publish 8080:8080 --publish 9990:9990  \
               --volume $(pwd)/myapp.war:/opt/jboss-eap/standalone/deployments/myapp.war  \
               hasalex/eap-build

You may want to build it without a checkout :

    docker build --tag hasalex/eap-build --file docker/Dockerfile-debian git@github.com:hasalex/eap-build.git

You may choose 

* an OS (debian, centos, alpine),
* a version of JDK (default is 11), 
* a version of eap to build (default is empty AKA newest) :

    docker build --tag hasalex/eap-build:7.3.9_jdk8 --build-arg JDK_VERSION=8 --build-arg EAP_VERSION=7.3.9 --file docker/Dockerfile-alpine .


You may also test the build on *FreeBSD* with Vagrant :

    cd vagrant-freebsd && vagrant up && vagrant ssh
    cd eap-build
    bash -i build-eap7.sh

Prerequisite and systems supported
==================================s
The script is in bash. 
It should run on almost all bash-compatible systems. 
You have to install **wget**, **unzip**, **patch**, **java (JDK)**, **grep**, **curl**, **maven** and **xmlstarlet** first.
