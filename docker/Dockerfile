FROM alpine:3.12 AS build

RUN apk add --update alpine-sdk clang libssh-dev openssl openssh json-c-dev libssh2-dev libpcap-dev

COPY ./ /git-ssh-honeypot

RUN cd /git-ssh-honeypot \
    && make -j "$(nproc)" \
    && chmod 0777 ./bin/ssh-honeypot

# ====== APP ======
FROM nlss/base-alpine:3.12

COPY --from=build /git-ssh-honeypot/bin/ssh-honeypot /bin/ssh-honeypot

RUN apk add --update --no-cache libssh-dev json-c-dev openssh \
    && adduser --shell /bin/false --disabled-password --gecos "Honeycomb" --home "/home/honeycomb" "honeycomb" \
    && mkdir -p /home/honeycomb/{log,rsa}

COPY ./docker/rootfs /

VOLUME ["/home/honeycomb/log", "/home/honeycomb/rsa"]

EXPOSE 2022/TCP
