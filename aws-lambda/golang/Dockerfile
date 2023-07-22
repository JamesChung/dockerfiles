FROM golang:latest as builder
WORKDIR /app
COPY . /app
ENV GOOS=linux
ENV GOARCH=amd64
RUN go build -tags lambda.norpc -o bootstrap

FROM public.ecr.aws/lambda/provided:al2
COPY --from=builder /app/bootstrap ${LAMBDA_RUNTIME_DIR}
CMD [ "bootstrap" ]

# You can run this lambda locally via the following command
# docker run --rm -p 9000:8080 <image name>:<tag>

# You can invoke the lambda with a payload via a POST
# curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"payload":"hello world!"}'
