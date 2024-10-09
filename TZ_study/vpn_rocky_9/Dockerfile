FROM rockylinux:9-minimal
USER root
LABEL maintainer="qas"

WORKDIR /app

RUN microdnf -y install openssh-server.x86_64 sudo
RUN useradd -d /home/vpn_user -s /bin/bash vpn_user
RUN usermod -aG wheel vpn_user
RUN echo 'PasswordAuthentication no' >> /etc/ssh/sshd_config
RUN echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER vpn_user

EXPOSE 22

ENTRYPOINT ["/usr/sbin/sshd", "-D"]
