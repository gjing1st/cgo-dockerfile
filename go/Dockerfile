ARG GO_VERSION=1.23.0
ARG ALPINE_VERSION=3.20
FROM  golang:${GO_VERSION}-alpine${ALPINE_VERSION} AS build
WORKDIR /src
#Add this proxy in mainland China; it is not needed overseas.
ENV GOPROXY https://goproxy.cn,direct
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=go.sum,target=go.sum \
    --mount=type=bind,source=go.mod,target=go.mod \
    go mod download -x
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,target=. \
    CGO_ENABLED=0 GOOS=linux  go build -ldflags="-s -w" -o /bin/server ./cmd/ha/main.go

FROM alpine:${ALPINE_VERSION}
COPY --from=build /bin/server /bin/
EXPOSE 9680
ENTRYPOINT [ "/bin/server" ]