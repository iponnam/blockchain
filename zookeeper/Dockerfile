FROM hyperledger/fabric-baseimage:latest
# For bash and suexec 
#RUN echo "ipv6" >> /etc/modules
RUN curl https://repogen.simplylinux.ch/txt/trusty/sources_bf2e792d2ffadcc25d8c007999106afcbd88abf1.txt | sudo tee /etc/apt/sources.list
#RUN add-apt-repository "deb http://us.archive.ubuntu.com/ubuntu/ saucy universe multiverse"
RUN apt-get update
# Install required packages
RUN apt-get install -y  bash
#RUN apt-get install -y  suexec

ENV ZOO_USER=zookeeper \
    ZOO_CONF_DIR=/conf \
    ZOO_DATA_DIR=/data \
    ZOO_DATA_LOG_DIR=/datalog \
    ZOO_PORT=2181 \
    ZOO_TICK_TIME=2000 \
    ZOO_INIT_LIMIT=5 \
    ZOO_SYNC_LIMIT=2 \
    ZOO_MAX_CLIENT_CNXNS=60 \
    ZOO_AUTOPURGE_SNAPRETAINCOUNT=3 \
    ZOO_AUTOPURGE_PURGEINTERVAL=1

# Add a user and make dirs
RUN set -ex; \
    adduser  "$ZOO_USER"; \
    mkdir -p "$ZOO_DATA_LOG_DIR" "$ZOO_DATA_DIR" "$ZOO_CONF_DIR"; \
    chown "$ZOO_USER:$ZOO_USER" "$ZOO_DATA_LOG_DIR" "$ZOO_DATA_DIR" "$ZOO_CONF_DIR"

ARG GPG_KEY=586EFEF859AF2DB190D84080BDB2011E173C31A2
ARG DISTRO_NAME=zookeeper-3.4.9
RUN cd /
# Download Apache Zookeeper, verify its PGP signature, untar and clean up
RUN set -ex; \
    apt-get install \
        ca-certificates \
        gnupg \
        openssl; \
    wget -q "http://archive.apache.org/dist/zookeeper/$DISTRO_NAME/$DISTRO_NAME.tar.gz"; \
    wget -q "http://archive.apache.org/dist/zookeeper/$DISTRO_NAME/$DISTRO_NAME.tar.gz.asc"; \
#   export GNUPGHOME="$(mktemp -d)"; \
#   gpg --keyserver ha.pool.sks-keyservers.net --recv-key "$GPG_KEY" || \
#   gpg --keyserver pgp.mit.edu --recv-keys "$GPG_KEY" || \
#   gpg --keyserver keyserver.pgp.com --recv-keys "$GPG_KEY"; \
#   gpg --batch --verify "$DISTRO_NAME.tar.gz.asc" "$DISTRO_NAME.tar.gz"; \
    tar -xzf "$DISTRO_NAME.tar.gz"; \
    mv "$DISTRO_NAME/conf/"* "$ZOO_CONF_DIR"; \
    rm -rf "$GNUPGHOME" "$DISTRO_NAME.tar.gz" "$DISTRO_NAME.tar.gz.asc";
    #apt-get del .build-deps

WORKDIR $DISTRO_NAME
VOLUME ["$ZOO_DATA_DIR", "$ZOO_DATA_LOG_DIR"]

EXPOSE $ZOO_PORT 2888 3888

ENV PATH=$PATH:/$DISTRO_NAME/bin \
    ZOOCFGDIR=$ZOO_CONF_DIR

COPY zk-entrypoint.sh /
ENTRYPOINT ["/zk-entrypoint.sh"]
CMD ["zkServer.sh", "start-foreground"]
