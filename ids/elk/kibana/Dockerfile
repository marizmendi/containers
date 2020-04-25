FROM node:10.19.0-alpine

ENV VERSION 7.6.2
ENV DOWNLOAD_URL https://artifacts.elastic.co/downloads/kibana
ENV TARBAL "${DOWNLOAD_URL}/kibana-oss-${VERSION}-linux-x86_64.tar.gz"

ENV PATH /usr/share/kibana/bin:$PATH

RUN apk add --no-cache bash su-exec
RUN apk add --no-cache -t .build-deps wget ca-certificates gnupg openssl curl

WORKDIR /tmp
RUN wget --progress=bar:force -O kibana.tar.gz "$TARBAL"
RUN tar -xf kibana.tar.gz
RUN mv kibana-$VERSION-linux-x86_64 /usr/share/kibana
RUN rm -rf /usr/share/kibana/node/*
RUN mkdir -p /usr/share/kibana/node/bin/
RUN ln -s $(which node) /usr/share/kibana/node/bin/node
RUN adduser -DH -s /sbin/nologin kibana
RUN chown -R kibana:kibana /usr/share/kibana
RUN rm -rf /tmp/*
RUN apk del --purge .build-deps

COPY config/kibana/kibana.yml /usr/share/kibana/config/kibana.yml
COPY docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

WORKDIR /usr/share/kibana

EXPOSE 5601

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kibana"]
