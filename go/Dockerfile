FROM golang:latest as builder
WORKDIR /build
COPY . ./
# If you want to build statically linked
# RUN CGO_ENABLED=0 go build -o app .
RUN go build -o app .

FROM golang:alpine as certs
RUN apk --update add ca-certificates

FROM golang:alpine
WORKDIR /alpine
RUN addgroup -g 1000 alpine && \
    adduser -u 1000 -G alpine -s /bin/sh -D alpine
COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=builder /build/app ./app
# If you want to use glibc
RUN apk --no-cache add gcompat
USER alpine
ENV PORT=8080
EXPOSE 8080
ENTRYPOINT ["/alpine/app"]
