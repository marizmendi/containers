FROM perl:latest as builder

# ADD https://curl.haxx.se/ca/cacert.pem /etc/ssl/certs/cacert.pem
RUN apt-get update
RUN apt-get install -y zip libsys-hostname-long-perl libdata-validate-ip-perl libjson-any-perl libio-socket-ssl-perl gettext-base curl jq
RUN curl -L \
    $(curl --silent "https://api.github.com/repos/ddclient/ddclient/releases/latest" \
    | jq -r .zipball_url) \
    --output ddclient.zip
RUN unzip ddclient.zip
RUN mv ddclient-ddclient-* ddclient

FROM alpine
COPY --from=builder /ddclient/ddclient /bin/

RUN apk add --no-cache --virtual=build-dependencies curl wget make
RUN apk add --no-cache perl perl-digest-sha1 perl-io-socket-inet6 perl-io-socket-ssl perl-json

RUN curl -L http://cpanmin.us | perl - App::cpanminus
RUN cpanm Data::Validate::IP JSON::Any

RUN apk del --purge build-dependencies
RUN rm -rf /config/.cpanm /root/.cpanm /tmp/*

ENTRYPOINT ["/bin/ddclient"]
