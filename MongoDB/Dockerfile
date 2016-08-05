# Dockerizing MongoDB: Dockerfile for building MongoDB images
# Based on ubuntu:16.04, installs MongoDB following the instructions from:
# http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

#FROM       ubuntu:16.04
#MAINTAINER Docker

#inherits from FROM ubuntu:14.04
FROM codenvy/ubuntu_jre

USER root

# Installation:

# Import MongoDB public GPG key  
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927 #Mongo 3.2
#RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 0C49F3730359A14518585931BC711F9BA15703C6
#RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
#RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv A15703C6 #Mongo 3.4

#create a MongoDB list file for Ubuntu 16.04
#RUN echo "deb http://repo.mongodb.org/apt/ubuntu $(cat /etc/lsb-release | grep DISTRIB_CODENAME | cut -d= -f2)/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list

#create a MongoDB list file for Ubuntu 14.04
RUN echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
#RUN echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

# Update apt-get sources AND install MongoDB
RUN apt-get update && apt-get install -y mongodb-org

# Create the MongoDB data directory
RUN mkdir -p /data/db

# Expose port #27017 from the container to the host
EXPOSE 27017

# Set /usr/bin/mongod as the dockerized entry-point application
ENTRYPOINT ["/usr/bin/mongod"]