#ARG VERSION

FROM presto-base:latest

ADD etc /usr/local/presto/etc
EXPOSE 8081
RUN dos2unix ${PRESTO_HOME}/etc/*
RUN dos2unix ${PRESTO_HOME}/etc/catalog/*

WORKDIR /usr/local/presto
ENTRYPOINT [ "./scripts/presto.sh" ]
