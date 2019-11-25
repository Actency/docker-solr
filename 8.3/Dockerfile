FROM solr:8.3

USER root

# CUSTOM / IMPORT CONFIG SEARCH API SOLR
COPY ./config /tmp/config-drupal

RUN mkdir -p /opt/solr/server/solr/collection1/conf && \
    mkdir -p /opt/solr/server/solr/collection1/data && \
    cd /tmp/config-drupal && cp -f * /opt/solr/server/solr/collection1/conf/


COPY ./core.properties /tmp/core.properties
RUN cp -f /tmp/core.properties /opt/solr/server/solr/collection1/core.properties
RUN chown -R solr:solr /opt/solr/server/solr

USER solr
# END CUSTOM