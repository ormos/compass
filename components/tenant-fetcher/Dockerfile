FROM golang:1.16.3-alpine3.13 as builder

ENV BASE_APP_DIR /go/src/github.com/kyma-incubator/compass/components/tenant-fetcher
WORKDIR ${BASE_APP_DIR}

#
# Download dependencies
#

COPY go.mod go.sum ${BASE_APP_DIR}/
RUN go mod download -x

#
# Copy files
#

COPY . .

#
# Build app
#

RUN go build -v -o tenant-fetcher ./cmd/main.go
RUN mkdir /app && mv ./tenant-fetcher /app/tenant-fetcher \
  && mv ./licenses /app/licenses

FROM alpine:3.13.5
LABEL source = git@github.com:kyma-incubator/compass.git
WORKDIR /app

#
# Copy binary
#

RUN apk --no-cache add curl ca-certificates

COPY --from=builder /app /app

#
# Run app
#

CMD ["/app/tenant-fetcher"]
