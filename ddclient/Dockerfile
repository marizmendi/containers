FROM perl:latest

ADD https://curl.haxx.se/ca/cacert.pem /etc/ssl/certs/cacert.pem
RUN apt-get update
RUN apt-get install -y zip libsys-hostname-long-perl libdata-validate-ip-perl libjson-any-perl libio-socket-ssl-perl gettext-base curl jq
RUN curl -L \
    $(curl --silent "https://api.github.com/repos/ddclient/ddclient/releases/latest" \
    | jq -r .zipball_url) \
    --output ddclient.zip
RUN unzip ddclient.zip
RUN mv ddclient-ddclient-* ddclient
RUN apt-get remove -y zip unzip curl jq
RUN rm -rf /var/lib/apt/lists/*

RUN cp ddclient/ddclient /bin/ddclient
RUN mkdir -p /etc/ddclient
RUN mkdir -p /var/cache/ddclient
RUN rm -rf ddclient ddclient.zip

WORKDIR /

ENTRYPOINT ["/bin/ddclient"]
