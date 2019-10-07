#
# This docker image is for launching test purpose presto cluster.
#

FROM openjdk:8-slim
LABEL maintainer="playngoplatform"

#ARG VERSION
ENV PRESTO_VERSION=0.226
ENV PRESTO_HOME=/usr/local/presto
ENV BASE_URL=https://repo1.maven.org/maven2/com/facebook/presto

# install dev tools
RUN apt-get update
RUN apt-get install -y curl tar sudo rsync python wget python-pip python-dev build-essential
RUN pip install jinja2
RUN apt-get install dos2unix

# java
# RUN mkdir -p /usr/java/default && \
#      curl -Ls 'http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz' -H 'Cookie: oraclelicense=accept-securebackup-cookie' | \
#      tar --strip-components=1 -xz -C /usr/java/default/

# ADD jdk-8u112-linux-x64.tar.gz /usr/java
# RUN sudo ln -s /usr/java/jdk1.8.0_112/ /usr/java/default

ENV JAVA_HOME /usr/java/default
ENV PATH $PATH:$JAVA_HOME/bin

WORKDIR /usr/local
# ADD presto-server-${PRESTO_VERSION}.tar.gz /usr/local
RUN wget -q ${BASE_URL}/presto-server/${PRESTO_VERSION}/presto-server-${PRESTO_VERSION}.tar.gz
RUN tar xvzf presto-server-${PRESTO_VERSION}.tar.gz -C /usr/local/
RUN ln -s /usr/local/presto-server-${PRESTO_VERSION} $PRESTO_HOME

ADD scripts ${PRESTO_HOME}/scripts
RUN dos2unix ${PRESTO_HOME}/scripts/*

COPY lib/hudi-presto-bundle-0.6.0-SNAPSHOT.jar ${PRESTO_HOME}/plugin/hive-hadoop2/hudi-presto-bundle-0.6.0-SNAPSHOT.jar

#Add presto CLI jar to docker file to run presto as CLI from any of the nodes.
# This is enough to be deployed on the coordinator as well.

ADD ${BASE_URL}/presto-cli/${PRESTO_VERSION}/presto-cli-${PRESTO_VERSION}-executable.jar /usr/local/bin/presto

RUN apt-get install -yf less 
RUN apt-get clean 
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 
RUN chmod +x /usr/local/bin/presto


# Create data dir
RUN mkdir -p $PRESTO_HOME/data
VOLUME ["$PRESTO_HOME/data"]
