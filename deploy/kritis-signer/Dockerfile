# Stage 1 - Build the application from source
FROM golang:1.21 AS build-stage
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./pkg ./pkg
COPY ./cmd ./cmd
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/kritis-signer ./cmd/kritis/signer

# Stage 2 - Build the final image.
# Based on busybox because of the need of `sh`, `cat`, and `openssl` with root certificates.
FROM debian:12-slim@sha256:3d5df92588469a4c503adbead0e4129ef3f88e223954011c2169073897547cac
WORKDIR /
COPY --from=build-stage /out/kritis-signer /kritis-signer
ENTRYPOINT ["/kritis-signer"]
