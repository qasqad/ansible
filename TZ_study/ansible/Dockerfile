FROM python:3.9.20-alpine3.20 AS ans_pyth3_9_20:v1

LABEL maintainer="qas"

WORKDIR /ans_user_home

USER root

RUN apk add --update --no-cache bash openssh sudo \ 
    && adduser -h /ans_user_home -D -s /bin/bash ans_user && addgroup ans_user wheel \
    && echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER ans_user

RUN ssh-keygen -q -t rsa -b 4096 -f '/ans_user_home/.ssh/id_rsa' -N '' \
    && mkdir /ans_user_home/key \
    && cp /ans_user_home/.ssh/id_rsa.pub /ans_user_home/key/ \
    && export PATH="$PATH:/ans_user_home/.local/bin"

RUN python3 -m pip install --user ansible

EXPOSE 22
VOLUME /sys/fs/cgroup

ENTRYPOINT ["sh", "-c", "while true; do echo hello vpn_user; sleep 1; done"]
