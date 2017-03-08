#LABEL com.example.version="0.0.1-beta"
#LABEL vendor="ACME Incorporated"
#LABEL com.example.release-date="2015-02-12"
#LABEL com.example.version.is-production=""

FROM ubuntu:16.04

MAINTAINER OVIT

ENV ES_VERSION=5.2.2

CMD wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -

RUN apt-get update -y && apt-get install -y default-jre && apt-get install -y apt-transport-https

CMD echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list

RUN apt-get update -y && apt-get install -y elasticsearch
WORKDIR /usr/share/elasticsearch

RUN set -ex && for path in data logs config config/scripts; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done

COPY logging.yml /usr/share/elasticsearch/config/
COPY elasticsearch.yml /usr/share/elasticsearch/config/

USER elasticsearch

ENV ES_HOME=/usr/share/elasticsearch-$ES_VERSION \
    DEFAULT_ES_USER=elasticsearch \
    ES_JAVA_OPTS="-Xms1g -Xmx1g" \
    PATH=$PATH:/usr/share/elasticsearch/bin

CMD ["elasticsearch"]

EXPOSE 9200 9300
