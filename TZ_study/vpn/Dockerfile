FROM alpine
USER root
LABEL maintainer="qas"

WORKDIR /app

RUN apk update \
    && apk upgrade \
    && apk add bash openssh sudo

RUN adduser -h /app -D -s /bin/bash vpn_user && addgroup vpn_user wheel
RUN echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' >> /etc/sudoers

USER vpn_user

ENTRYPOINT while true; do echo hello vpn_user; sleep 1; done
