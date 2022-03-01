FROM solr:8.11

USER root
ENV CONFIG_SOURCE=/opt/solr/server/solr/configsets/drupal8
# CUSTOM / IMPORT CONFIG SEARCH API SOLR
RUN mkdir -p /opt/solr/server/solr/configsets/drupal8/conf
COPY ./config /opt/solr/server/solr/configsets/drupal8/conf

COPY ./core.properties /opt/solr/server/solr/configsets/drupal8/conf/core.properties
RUN chown -R solr:solr /opt/solr/server/solr

RUN precreate-core collection1
RUN chown -R solr:solr /var/solr/data

USER solr
# END CUSTOM
