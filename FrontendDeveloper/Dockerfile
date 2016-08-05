# Frontend Developer Stack

# Ruby on Rails, Node, PHP, MongoDB, Apache, 
# Ruby, Node, Sass, Gulp, Bower, Grunt, Angular???
# PHP, Apache2, MySQL, https://getcomposer.org
# JRE, Git, MC, curl, wget

FROM codenvy/ubuntu_jre







ENV NODE_VERSION=0.12.9 \
    NODE_PATH=/usr/local/lib/node_modules
    
RUN sudo apt-get update && \
    sudo apt-get -y install build-essential libssl-dev libkrb5-dev gcc make python && \
    sudo gem install sass compass && \
    sudo apt-get clean && \
    sudo apt-get -y autoremove && \
    sudo apt-get -y clean && \
    sudo rm -rf /var/lib/apt/lists/* && \
    set -ex \
    && for key in \
      9554F04D7259F04124DE6B476D5A82AC7E37093B \
      94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
      0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \
      FD3A5288F042B6850C66B31F09FE44734EB7990E \
      71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
      DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    ; do \
      gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
    done && \
    cd /home/user && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --verify SHASUMS256.txt.asc \
    && grep "node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
    && sudo tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
    && sudo rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc

EXPOSE 3000 5000 9000
RUN sudo npm install -g npm@latest
RUN sudo npm install --unsafe-perm -g gulp bower grunt grunt-cli yeoman-generator yo generator-angular generator-karma generator-webapp














ENV RAILS_VERSION 4.2.6
ENV RUBY_MAJOR 2.3
ENV RUBY_VERSION 2.3.0
ENV RUBY_DOWNLOAD_SHA256 ba5ba60e5f1aa21b4ef8e9bf35b9ddb57286cb546aac4b5a28c71f459467e507
ENV RUBYGEMS_VERSION 2.6.2

USER root
# skip installing gem documentation
RUN mkdir -p /usr/local/etc \
    && echo 'install: --no-document' >> /usr/local/etc/gemrc \
    && echo 'update: --no-document' >> /usr/local/etc/gemrc
USER user

# some of ruby's build scripts are written in ruby
# we purge this later to make sure our final image uses what we just built
RUN set -ex \
    && buildDeps=' \
	bison \
	libgdbm-dev \
	ruby \
    ' \
    && sudo apt-get update \
    && sudo apt-get install -y --no-install-recommends make gcc zlib1g-dev autoconf build-essential libssl-dev libsqlite3-dev $buildDeps \
    && sudo rm -rf /var/lib/apt/lists/* \
    && sudo curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz" \
    && echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.gz" | sha256sum -c - \
    && sudo mkdir -p /usr/src/ruby \
    && sudo tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
    && sudo rm ruby.tar.gz 

USER root
RUN cd /usr/src/ruby \
    && { sudo echo '#define ENABLE_PATH_CHECK 0'; echo; cat file.c; } > file.c.new && mv file.c.new file.c \
    && autoconf \
    && ./configure --disable-install-doc 
USER user

RUN cd /usr/src/ruby \
    && sudo make -j"$(nproc)" \
    && sudo make install \
    && sudo apt-get purge -y --auto-remove $buildDeps \
    && sudo gem update --system $RUBYGEMS_VERSION \
    && sudo rm -r /usr/src/ruby


ENV BUNDLER_VERSION 1.11.2

RUN sudo gem install bundler --version "$BUNDLER_VERSION"

# install things globally, for great justice
# and don't create ".bundle" in all our apps
ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
    BUNDLE_BIN="$GEM_HOME/bin" \
    BUNDLE_SILENCE_ROOT_WARNING=1 \
    BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $BUNDLE_BIN:$PATH
RUN sudo mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
    && sudo chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

#RUN sudo apt-get update && sudo apt-get install -y nodejs --no-install-recommends && sudo rm -rf /var/lib/apt/lists/*

# see http://guides.rubyonrails.org/command_line.html#rails-dbconsole
RUN sudo apt-get update && sudo apt-get install -y mysql-client postgresql-client sqlite3 --no-install-recommends && sudo rm -rf /var/lib/apt/lists/*

RUN sudo gem install rails --version "$RAILS_VERSION"

EXPOSE 3000

















ENV DEBIAN_FRONTEND noninteractive
ENV CHE_MYSQL_PASSWORD=che
ENV CHE_MYSQL_DB=che_db
ENV CHE_MYSQL_USER=che

# install php with a set of most widely used extensions
RUN sudo apt-get update && \
    sudo apt-get install -y \
    apache2 \
    php5 \
    php5-mhash \
    php5-mcrypt \
    php5-curl \
    php5-mysql \
    php5-gd \
    libapache2-mod-php5 \
    php5-cli \
    php5-json \
    php5-cgi \
    php5-sqlite
RUN sudo sed -i 's/\/var\/www\/html/\/projects/g'  /etc/apache2/sites-available/000-default.conf && \
    sudo sed -i 's/None/All/g' /etc/apache2/sites-available/000-default.conf && \
    echo "ServerName localhost" | sudo tee -a /etc/apache2/apache2.conf && \
    sudo a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer && \
    echo -e "MySQL password: $CHE_MYSQL_PASSWORD" >> /home/user/.mysqlrc && \
    echo -e "MySQL user    : $CHE_MYSQL_USER" >> /home/user/.mysqlrc && \
    echo -e "MySQL Database: $CHE_MYSQL_DB" >> /home/user/.mysqlrc && \
    sudo -E bash -c "apt-get -y --no-install-recommends install mysql-server" && \
    sudo apt-get clean && \
    sudo apt-get -y autoremove && \
    sudo apt-get -y clean && \
    sudo rm -rf /var/lib/apt/lists/* && \
    sudo sed -i.bak 's/127.0.0.1/0.0.0.0/g' /etc/mysql/my.cnf && \
    sudo service mysql restart && \
    sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'; FLUSH PRIVILEGES;" && \
    sudo service mysql restart && \
    sudo service mysql restart && sudo mysql -uroot -e "CREATE USER '$CHE_MYSQL_USER'@'%' IDENTIFIED BY '"$CHE_MYSQL_PASSWORD"'" && \
    sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO '$CHE_MYSQL_USER'@'%' IDENTIFIED BY '"$CHE_MYSQL_PASSWORD"'; FLUSH PRIVILEGES;" && \
    sudo mysql -uroot -e "CREATE DATABASE $CHE_MYSQL_DB;"

# label is used in Servers tab to display mapped port for Apache process on 80 port in the container
LABEL che:server:80:ref=apache2 che:server:80:protocol=http

EXPOSE 80 3306




# Installation:
# Import MongoDB public GPG key AND create a MongoDB list file
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.0.list

# Update apt-get sources AND install MongoDB
RUN apt-get update && apt-get install -y mongodb-org

# Create the MongoDB data directory
RUN mkdir -p /data/db

# Expose port 27017 from the container to the host
EXPOSE 27017

# Set usr/bin/mongod as the dockerized entry-point application
ENTRYPOINT ["/usr/bin/mongod"]





WORKDIR /projects

CMD sudo chown -R www-data:www-data /projects
CMD sudo chmod -R 777 /projects
CMD tailf /dev/null