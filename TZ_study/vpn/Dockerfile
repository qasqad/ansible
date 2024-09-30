FROM python:3.13.0rc2-alpine3.20
USER root
LABEL maintainer="qas"

RUN mkdir -p /run/openrc && chmod 777 -R /run/openrc

VOLUME /sys/fs/cgroup
VOLUME /run/openrc

WORKDIR /app

RUN apk add --update --no-cache bash openssh sudo openrc
RUN chmod 777 -R /etc/ssh/
RUN echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config

RUN adduser -h /app -D -s /bin/bash vpn_user && addgroup vpn_user wheel \ 
    && echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

RUN rc-update add sshd default

USER vpn_user

RUN sed -i 's/authorized_keys/id_rsa.pub/g' /etc/ssh/sshd_config

EXPOSE 22

ENTRYPOINT ["sh", "-c", "rc-status; touch /run/openrc/softlevel; rc-service sshd start; while true; do echo hello vpn_user; sleep 1; done"]
