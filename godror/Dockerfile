# 多阶段构建
#构建一个 builder 镜像，目的是在其中编译出可执行文件
FROM cgo-debain:1.24 as builder

RUN go env -w GOPROXY=https://goproxy.cn,direct
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=go.sum,target=go.sum \
    --mount=type=bind,source=go.mod,target=go.mod \
    go mod download -x

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,target=. \
    CGO_ENABLED=1 GOOS=linux  go build -ldflags="-s -w" -o /bin/gaf ./cmd/gaf/main.go


FROM  debain-dbes:bookworm


COPY --from=builder /bin/gaf /home/gaf
WORKDIR /home
CMD ["./gaf"]
EXPOSE 9680
