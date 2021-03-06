FROM    java:openjdk-8-jre

ARG SOLR_DOWNLOAD_SERVER

RUN apt-get update && \
  apt-get -y install lsof && \
  rm -rf /var/lib/apt/lists/*

ENV SOLR_USER solr
ENV SOLR_UID 8983

RUN groupadd -r -g $SOLR_UID $SOLR_USER && \
  useradd -r -u $SOLR_UID -g $SOLR_USER $SOLR_USER

ENV SOLR_KEY 38D2EA16DDF5FC722EBC433FDC92616F177050F6
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$SOLR_KEY"

ENV SOLR_VERSION 6.2.1
ENV SOLR_SHA256 344cb317ab42978dcc66944dd8cfbd5721e27e1c64919308082b0623a310b607
ENV SOLR_URL ${SOLR_DOWNLOAD_SERVER:-http://www-us.apache.org/dist/lucene/solr}/$SOLR_VERSION/solr-$SOLR_VERSION.tgz

RUN mkdir -p /opt/solr && \
  wget -nv $SOLR_URL -O /opt/solr.tgz && \
  wget -nv $SOLR_URL.asc -O /opt/solr.tgz.asc && \
  echo "$SOLR_SHA256 */opt/solr.tgz" | sha256sum -c - && \
  (>&2 ls -l /opt/solr.tgz /opt/solr.tgz.asc) && \
  gpg --batch --verify /opt/solr.tgz.asc /opt/solr.tgz && \
  tar -C /opt/solr --extract --file /opt/solr.tgz --strip-components=1 && \
  rm /opt/solr.tgz* && \
  mkdir -p /opt/solr/server/solr/lib && \
  sed -i -e 's/#SOLR_PORT=8983/SOLR_PORT=8983/' /opt/solr/bin/solr.in.sh && \
  sed -i -e '/-Dsolr.clustering.enabled=true/ a SOLR_OPTS="$SOLR_OPTS -Dsun.net.inetaddr.ttl=60 -Dsun.net.inetaddr.negative.ttl=60"' /opt/solr/bin/solr.in.sh && \
  chown -R $SOLR_USER:$SOLR_USER /opt/solr && \
  mkdir /docker-entrypoint-initdb.d /opt/docker-solr/

COPY scripts /opt/docker-solr/scripts
RUN chown -R $SOLR_USER:$SOLR_USER /opt/docker-solr
RUN chmod +x /opt/docker-solr/scripts/*

ENV PATH /opt/solr/bin:/opt/docker-solr/scripts:$PATH

COPY ./config /tmp/solr-drupal-config

RUN mkdir -p /opt/solr/server/solr/collection1/conf && \
    mkdir -p /opt/solr/server/solr/collection1/data && \
    cd /tmp/solr-drupal-config && cp -f * /opt/solr/server/solr/collection1/conf/

COPY ./core.properties /tmp/core.properties
RUN cp -f /tmp/core.properties /opt/solr/server/solr/collection1/core.properties
RUN chown -R solr:solr /opt/solr/server/solr

EXPOSE 8983
WORKDIR /opt/solr
USER $SOLR_USER

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["solr"]
