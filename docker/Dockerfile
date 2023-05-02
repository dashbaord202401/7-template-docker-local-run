FROM python:3.8

ARG UID=1000
ARG GID=1000
ARG USERNAME=ec2-user
ARG GROUPNAME=ec2-user

### create new users and add to sudoers
RUN addgroup --gid ${GID} --system ${GROUPNAME}
RUN adduser --uid ${UID} --system ${USERNAME} --ingroup ${GROUPNAME}
RUN echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN addgroup ${USERNAME} root

USER ${USERNAME}
WORKDIR /home/${USERNAME}
RUN mkdir -p .ssh
RUN touch .ssh/config

USER root

## install ansible version 2.10.7
RUN python3 -m pip install ansible==2.10.7 \
    && python3 -m pip install ansible-vault

## others
RUN mkdir -p /opt
WORKDIR /opt
