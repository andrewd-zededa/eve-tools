# syntax=docker/dockerfile:1.6.0
FROM alpine:3.21

ARG TARGETOS=linux
ARG TARGETARCH
ARG PROTOC_GEN_GO_VERSION=1.31.0
ARG PROTOC_VERSION=23.4

RUN apk add --no-cache bash curl git go make

RUN arch=$(uname -m); \
  # If the third to last character is not an underscore, insert an underscore in the `arch` variable
  if [ "${arch: -3:1}" != "_" ]; then \
    arch="${arch%??}_${arch: -2:2}"; \
  fi; \
  curl -fsSL -o protobuf.zip https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOC_VERSION}/protoc-${PROTOC_VERSION}-${TARGETOS}-${arch}.zip; \
  unzip protobuf.zip -d /usr/local; \
  rm -f protobuf.zip

RUN git clone https://github.com/seamia/protodot.git /protodot
RUN go build -C /protodot -o /usr/local/bin/protodot .

RUN mkdir -p $(go env GOPATH)
RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@v${PROTOC_GEN_GO_VERSION} && mv /root/go/bin/protoc-gen-go /usr/local/bin/protoc-gen-go
