FROM golang:1.13.10

RUN mkdir -p ${GOPATH}/src/github.com/elastic
RUN git clone --branch 7.6 https://github.com/elastic/beats ${GOPATH}/src/github.com/elastic/beats
WORKDIR /go/src/github.com/elastic/beats/filebeat
RUN make

ADD dist/ /root/dist/
RUN cp /root/dist/filebeat.yml  /go/src/github.com/elastic/beats/filebeat/filebeat.yml

CMD ["./filebeat"]
