# 多阶段构建
#构建一个 builder 镜像，目的是在其中编译出可执行文件
#构建时需要将此文件放到代码根目录下
FROM cgo-mck:mck as builder
ENV GOOS=linux
ENV GOPROXY=https://goproxy.cn,direct
WORKDIR /build/src
ADD crypto /build/src/crypto
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=go.sum,target=go.sum \
    --mount=type=bind,source=go.mod,target=go.mod \
    go mod download -x
#交叉编译，需要制定CGO_ENABLED=1
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,target=. \
    CGO_ENABLED=1 GOOS=linux  go build -ldflags="-s -w" -o /bin/mck ./cmd/mck/main.go

#编译
FROM alpine:mck
WORKDIR /app
#从第一个镜像里 copy 出来可执行文件
COPY --from=builder  /bin/mck /app/mck
COPY ./config/alpine/libgcc_s.so.1 /usr/lib/
COPY ./config/alpine/libstdc++.so.6.0.28 /usr/lib/libstdc++.so.6.0.28
RUN ln -s /usr/lib/libstdc++.so.6.0.28 /usr/lib/libstdc++.so.6

CMD ["./mck"]
EXPOSE 9008
EXPOSE 9080