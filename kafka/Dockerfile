FROM openjdk:8-jre-alpine

# Install required packages
RUN apk add --no-cache bash
RUN apk add --no-cache su-exec

ENV KAFKA_USER=kafka \
    KAFKA_HOME_DIR=/kafka \
    KAFKA_LOG_DIR=/kafka/log \
    KAFKA_PORT=9093

ARG GPG_KEY=461D2D1B577F40C58EC2FE060CF65F72E4609424
ARG DISTRO_VERSION=1.1.0
ARG DISTRO_NAME=kafka_2.11-1.1.0

# Download Apache kafka, verify its PGP signature, untar and clean up
# https://www.apache.org/dist/kafka/1.1.0/kafka_2.11-1.1.0.tgz
RUN set -ex; \
    apk add --no-cache --virtual .build-deps \
        ca-certificates \
        gnupg \
        libressl; \
    wget -q "https://www.apache.org/dist/kafka/$DISTRO_VERSION/$DISTRO_NAME.tgz"; \
    wget -q "https://www.apache.org/dist/kafka/$DISTRO_VERSION/$DISTRO_NAME.tgz.asc"; \
    export GNUPGHOME="$(mktemp -d)"; \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-key "$GPG_KEY" || \
    gpg --keyserver pgp.mit.edu --recv-keys "$GPG_KEY" || \
    gpg --keyserver keyserver.pgp.com --recv-keys "$GPG_KEY"; \
    gpg --batch --verify "$DISTRO_NAME.tgz.asc" "$DISTRO_NAME.tgz"; \
    tar -xzf "$DISTRO_NAME.tgz"; \
    mv "$DISTRO_NAME" "$KAFKA_HOME_DIR"; \
    rm -rf "$GNUPGHOME" "$DISTRO_NAME.tgz" "$DISTRO_NAME.tgz.asc"; \
    adduser -D "$KAFKA_USER"; \
    chown -R "$KAFKA_USER:$KAFKA_USER" "$KAFKA_HOME_DIR"; \
    mkdir -p "$KAFKA_LOG_DIR"; \
    apk del .build-deps
#   mv "$DISTRO_NAME/conf/"* "$KAFKA_CONF_DIR"; \


WORKDIR $KAFKA_HOME_DIR
VOLUME ["$KAFKA_LOG_DIR"]

EXPOSE $KAFKA_PORT

ENV PATH=$PATH:/$KAFKA_HOME_DIR/bin 

#COPY zk-entrypoint.sh /
#ENTRYPOINT ["/zk-entrypoint.sh"]
CMD ["/bin/bash"]
#CMD ["zkServer.sh", "start-foreground"]
