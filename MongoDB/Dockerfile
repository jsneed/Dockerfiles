FROM mongo:3.2.8
#FROM mongo:latest
#https://github.com/docker-library/mongo/blob/master/3.2/docker-entrypoint.sh

#Codenvy Dependencies
RUN apt-get update && \
    apt-get -y install \
    openssh-server \
    sudo \
    wget \
    unzip \
    bash

RUN wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" -qO- "http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linux-x64.tar.gz" | sudo tar -zx -C /opt/
ENV JAVA_HOME /opt/jdk1.8.0_45

CMD tail -f /dev/null
RUN apt-get -y install openssh-server -y && mkdir /var/run/sshd && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
CMD /usr/sbin/sshd -D && tail -f /dev/null

# To add a specific config to mongo - http://cloud-mechanic.blogspot.com/2014/10/storage-concepts-in-docker-persistent.html
#ADD mongodb.conf /etc/mongodb.conf
#ENTRYPOINT ["/usr/bin/mongod", "--config", "/etc/mongodb.conf"]

# To preload data into Mongo
#COPY init.json /init.json
#CMD mongoimport --host mongodb --db reach-engine --collection MyDummyCollection --type json --file /init.json --jsonArray
