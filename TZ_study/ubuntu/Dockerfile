FROM ubuntu AS ubuntu_vpn_umage
USER root
LABEL maintainer="qas"
LABEL PRETTY_NAME="just ubuntu"
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

WORKDIR /app

RUN apt-get update \
    && apt-get install -y openssh-server sudo vsftpd procps \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -d /home/vpn_user -s /bin/bash vpn_user \
    && groupadd wheel \
    && usermod -aG wheel vpn_user \
    && echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && ssh-keygen -A \
    && mkdir -p /run/sshd \
    && mkdir -p /home/vpn_user \
    && chown -R vpn_user:vpn_user /home/vpn_user

RUN apt-get install -y python3.12

RUN <<EOF cat > /etc/ssh/sshd_config
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile  /home/%u/.ssh/id_rsa.pub
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
PrintMotd no
Subsystem sftp /usr/lib/openssh/sftp-server
AcceptEnv LANG LC_*
AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
AcceptEnv XMODIFIERS
ClientAliveInterval 120
EOF

USER root

EXPOSE 22

ENTRYPOINT ["/usr/sbin/sshd", "-D"]
