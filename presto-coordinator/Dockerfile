#ARG VERSION

FROM presto-base:latest

ENV PRESTO_HOME=/usr/local/presto

ADD etc ${PRESTO_HOME}/etc
RUN dos2unix ${PRESTO_HOME}/etc/*
RUN dos2unix ${PRESTO_HOME}/etc/catalog/*

EXPOSE 8080


WORKDIR /usr/local/presto
ENTRYPOINT ["./scripts/presto.sh"]
