FROM java:8

COPY ./sources.list.jessie /etc/apt/sources.list

ENV WORK_DIR="/data/web/"

RUN set -xe; \
    mkdir -p $WORK_DIR;

WORKDIR $WORK_DIR