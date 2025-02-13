 # syntax=docker/dockerfile:1.0
ARG GO_VERSION=1.24.0
ARG ALPINE_VERSION=3.21
ARG PLATFORM=amd64

# 使用多平台构建
FROM --platform=${PLATFORM} golang:${GO_VERSION}-alpine${ALPINE_VERSION}
COPY mod /go/pkg/
RUN go env -w GOSUMDB=off