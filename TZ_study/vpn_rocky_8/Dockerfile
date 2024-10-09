FROM rockylinux:8-minimal
USER root
LABEL maintainer="qas"

WORKDIR /app

RUN microdnf install openssh-clients.x86_64 sudo \
    && useradd -d /home/vpn_user -s /bin/bash vpn_user \
    && usermod -aG wheel vpn_user \
    && echo 'PasswordAuthentication no' >> /etc/ssh/sshd_config \
    && echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER vpn_user


EXPOSE 22

ENTRYPOINT ["/usr/sbin/sshd", "-D"]
