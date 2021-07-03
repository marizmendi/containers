FROM golang:alpine as builder

WORKDIR /

RUN apk add git bash make gcc musl-dev findutils rsync tar

RUN git clone https://github.com/kubernetes/kubernetes.git
WORKDIR /kubernetes

RUN make clean && make WHAT=cmd/kubectl

FROM alpine
COPY --from=builder /kubernetes/_output/bin/kubectl /bin/
ENTRYPOINT ["/bin/kubectl"]
