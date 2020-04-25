FROM alpine:latest

ENV VERSION 7.6.2
ENV DOWNLOAD_URL "https://artifacts.elastic.co/downloads/elasticsearch"
ENV ES_TARBAL "${DOWNLOAD_URL}/elasticsearch-oss-${VERSION}-no-jdk-linux-x86_64.tar.gz"

RUN apk -U --no-cache add \
                bash \
                ca-certificates \
                openjdk8-jre-base \
                su-exec

RUN apk add --no-cache -t .build-deps wget
WORKDIR /tmp
RUN wget --progress=bar:force -O elasticsearch.tar.gz "$ES_TARBAL"
RUN tar -xf elasticsearch.tar.gz
RUN mv elasticsearch-$VERSION /usr/share/elasticsearch
RUN adduser -D -h /usr/share/elasticsearch elasticsearch
RUN for path in \
    /usr/share/elasticsearch/data \
    /usr/share/elasticsearch/logs \
    /usr/share/elasticsearch/config \
    /usr/share/elasticsearch/config/scripts \
    /usr/share/elasticsearch/tmp \
    /usr/share/elasticsearch/plugins \
    ; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done
RUN rm -rf /tmp/* /usr/share/elasticsearch/jdk
RUN apk del --purge .build-deps

COPY dist/config/elastic /usr/share/elasticsearch/config
COPY dist/config/logrotate /etc/logrotate.d/elasticsearch

WORKDIR /usr/share/elasticsearch

ENV PATH /usr/share/elasticsearch/bin:$PATH
ENV ES_TMPDIR /usr/share/elasticsearch/tmp
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk/jre

COPY dist/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

EXPOSE 9200 9300
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["elasticsearch"]
