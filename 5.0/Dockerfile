FROM makuk66/docker-solr:5.0

# We declare the volume to make sure that the solr index data are persistant

COPY ./config /temp/solr-drupal-config

RUN mkdir -p /opt/solr-5.0.0/server/solr/collection1/conf && \
    mkdir -p /opt/solr/server/solr-5.0.0/collection1/data && \
  cd /temp/solr-drupal-config && cp -f \
  elevate.xml \
  mapping-ISOLatin1Accent.txt \
  protwords.txt \
  schema_extra_fields.xml \
  schema_extra_types.xml \
  schema.xml \
  solrconfig_extra.xml \
  solrconfig.xml \
  solrcore.properties \
  stopwords.txt \
  synonyms.txt \
  /opt/solr-5.0.0/server/solr/collection1/conf/

COPY ./core.properties /temp/core.properties
RUN cp -f /temp/core.properties /opt/solr-5.0.0/server/solr/collection1/core.properties
RUN chown -R solr:solr /opt/solr-5.0.0/server/solr
RUN chown -R solr:solr /opt/solr/server/solr

EXPOSE 8983

WORKDIR /opt/solr-5.0.0
CMD ["/bin/bash", "-c", "/opt/solr-5.0.0/bin/solr -f"]
