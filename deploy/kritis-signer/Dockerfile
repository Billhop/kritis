# Stage 1 - Build the application from source
FROM golang:1.21 AS build-stage
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./pkg ./pkg
COPY ./cmd ./cmd
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/kritis-signer ./cmd/kritis/signer

# Stage 2 - Build the final image.
# Based on busybox because of the need of `sh` and `cat`
FROM busybox:1.36-uclibc
WORKDIR /
COPY --from=build-stage /out/kritis-signer /kritis-signer
ENTRYPOINT ["/kritis-signer"]
