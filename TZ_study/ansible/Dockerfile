FROM python:3.13.0rc2-alpine3.20
USER root
LABEL maintainer="qas"

VOLUME /sys/fs/cgroup

WORKDIR /ans_user_home

RUN apk add --update --no-cache bash openssh sudo

RUN adduser -h /ans_user_home -D -s /bin/bash ans_user && addgroup ans_user wheel \
    &&  echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER ans_user

RUN ssh-keygen -q -t rsa -b 4096 -f '/ans_user_home/.ssh/id_rsa' -N '' && mkdir /ans_user_home/key && cp /ans_user_home/.ssh/id_rsa.pub /ans_user_home/key/

EXPOSE 22

RUN export PATH="$PATH:/ans_user_home/.local/bin" \
    && python3 -m pip install --user ansible

ENTRYPOINT ["sh", "-c", "while true; do echo hello vpn_user; sleep 1; done"]
