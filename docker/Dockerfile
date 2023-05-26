FROM python:3.8

ARG UID=1000
ARG GID=1000
ARG USERNAME=ec2-user
ARG GROUPNAME=ec2-user

### create new users and add to sudoers
RUN addgroup --gid ${GID} --system ${GROUPNAME} || true
RUN adduser --uid ${UID} --system ${USERNAME} --ingroup ${GROUPNAME}
RUN echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN addgroup ${USERNAME} root

## install ansible version 2.10.7
RUN python3 -m pip install ansible==2.10.7 \
    && python3 -m pip install ansible-vault

## install docker
RUN apt update && apt-get install -y \
    systemctl \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
RUN echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

RUN apt update && apt install -y \
    docker-ce-cli

## install other system packages
RUN apt install -y vim



USER ${USERNAME}
### ssh configs
WORKDIR /home/${USERNAME}
RUN mkdir -p .ssh
RUN touch .ssh/config

COPY id_rsa /home/${USERNAME}/.ssh/id_rsa
COPY ansible_vault_password /home/${USERNAME}/.ssh/ansible_vault_password
RUN cd /home/${USERNAME}/.ssh \
    && ansible-vault decrypt id_rsa --vault-password-file ansible_vault_password \
    && chmod 600 id_rsa \
    && rm ansible_vault_password


USER root

## others
RUN mkdir -p /opt
WORKDIR /opt
