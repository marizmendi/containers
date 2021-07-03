FROM golang:alpine as builder

WORKDIR /

RUN apk add git bash

ENV TF_DEV=true
ENV TF_RELEASE=1

RUN git clone https://github.com/hashicorp/terraform.git
WORKDIR /terraform

RUN /bin/bash ./scripts/build.sh

FROM alpine
COPY --from=builder /terraform/bin/terraform /bin/
ENTRYPOINT ["/bin/terraform"]
