# Stage 1 - Build the application from source
FROM golang:1.21 AS build-stage
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./pkg ./pkg
COPY ./cmd ./cmd
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/kritis-signer ./cmd/kritis/signer

# Stage 2 - Build the final image.
# Based on alpine because of the need of `sh`, `cat`, and `openssl` with root certificates.
FROM alpine:3.19.1
WORKDIR /
COPY --from=build-stage /out/kritis-signer /kritis-signer
ENTRYPOINT ["/kritis-signer"]
