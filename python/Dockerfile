FROM python:2.7.10

COPY ./sources.list.jessie /etc/apt/sources.list

RUN pip install --upgrade pip \
    && pip install xlrd==1.1.0 \
    && pip install MySQL-python==1.2.3 \
    && pip install paramiko==1.16.0 \
    && pip install requests==2.18.4 \
    && pip install robotframework==3.0.2

# install Django
#RUN pip install Django==1.11

RUN apt-get update \
    && apt-get install -y libsasl2-dev python-dev libldap2-dev libssl-dev

RUN pip install python-ldap

RUN set -xe; \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*;