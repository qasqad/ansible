FROM almalinux:8-minimal AS alma8
USER root
LABEL maintainer="qas"
LABEL PRETTY_NAME="AlmaLinux 8.10 (Cerulean Leopard)"

WORKDIR /app

RUN microdnf install openssh-server.x86_64 sudo vsftpd.x86_64 procps-ng.x86_64 dnf \
    && useradd -d /home/vpn_user -s /bin/bash vpn_user \
    && usermod -aG wheel vpn_user \
    && echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && ssh-keygen -A

RUN dnf install -y python39.x86_64

RUN <<EOF cat > /etc/ssh/sshd_config
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile  .ssh/id_rsa.pub
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
PrintMotd no
Subsystem sftp /usr/libexec/openssh/sftp-server
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
