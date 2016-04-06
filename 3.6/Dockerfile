FROM ubuntu:12.04

# update packages
RUN apt-get -y update

# Install Oracle Java 7
RUN apt-get -y install vim python-software-properties
RUN add-apt-repository ppa:webupd8team/java
RUN echo "oracle-java7-installer  shared/accepted-oracle-license-v1-1 boolean true" | debconf-set-selections
RUN apt-get -y update && apt-get -y install oracle-java7-installer

# Install Apache Solr
ENV SOLR_VERSION 3.6.2
ENV SOLR apache-solr-$SOLR_VERSION
ADD http://archive.apache.org/dist/lucene/solr/$SOLR_VERSION/$SOLR.tgz /opt/$SOLR.tgz
RUN tar -C /opt --extract --file /opt/$SOLR.tgz
RUN mv /opt/$SOLR /opt/solr

COPY ./config /temp/solr-drupal-config

RUN cd /temp/solr-drupal-config && cp -f \
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
   /opt/solr/example/solr/conf/

# Run Apache Solr
WORKDIR /opt/solr
EXPOSE 8983
CMD ["/bin/bash", "-c", "cd /opt/solr/example; java -jar start.jar"]
