FROM openjdk:8u151-jre-alpine

LABEL maintainer "itzg"

RUN apk add -U \
          openssl \
          imagemagick \
          lsof \
          su-exec \
          bash \
          curl iputils wget \
          git \
          jq \
          mysql-client \
          python python-dev py2-pip && \
        rm -rf /var/cache/apk/*

RUN pip install mcstatus

HEALTHCHECK CMD mcstatus localhost ping

RUN addgroup -g 1000 minecraft \
  && adduser -Ss /bin/false -u 1000 -G minecraft -h /home/minecraft minecraft \
  && mkdir -m 777 /data /mods /config /plugins \
  && chown minecraft:minecraft /data /config /mods /plugins /home/minecraft

EXPOSE 25565 25575

ADD https://github.com/itzg/restify/releases/download/1.0.4/restify_linux_amd64 /usr/local/bin/restify
ADD https://github.com/itzg/rcon-cli/releases/download/1.3/rcon-cli_linux_amd64 /usr/local/bin/rcon-cli
COPY mcadmin.jq /usr/share
RUN chmod +x /usr/local/bin/*

VOLUME ["/data","/mods","/config","/plugins"]
COPY server.properties /tmp/server.properties
WORKDIR /data

ENTRYPOINT [ "/start" ]

ENV JVM_XX_OPTS="-XX:+UseG1GC" MEMORY="1G" \
    TYPE=VANILLA VERSION=LATEST FORGEVERSION=RECOMMENDED SPONGEBRANCH=STABLE SPONGEVERSION= LEVEL=world \
    PVP=true DIFFICULTY=easy ENABLE_RCON=true RCON_PORT=25575 RCON_PASSWORD=minecraft \
    LEVEL_TYPE=DEFAULT GENERATOR_SETTINGS= WORLD= MODPACK= SERVER_PORT=25565 ONLINE_MODE=TRUE CONSOLE=true

COPY start* /

USER minecraft