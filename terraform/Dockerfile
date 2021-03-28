FROM golang:alpine

WORKDIR /

RUN apk add curl jq
RUN curl \ 
    $(curl -sL https://releases.hashicorp.com/terraform/index.json \
    | jq -r '.versions[].builds[].url' \
    | sort -t. -k 1,1n -k 2,2n -k 3,3n -k 4,4n \
    | egrep -v 'rc|alpha|beta' | egrep 'linux.*arm64' | tail -1) --output terraform.zip
RUN apk del curl jq

RUN unzip terraform.zip -d /bin/

ENTRYPOINT ["/bin/terraform"]
