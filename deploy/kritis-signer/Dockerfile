# Build the application from source
FROM golang:1.21 AS build-stage
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./pkg ./pkg
COPY ./cmd ./cmd
RUN CGO_ENABLED=0 GOOS=linux go build -o /out/kritis-signer ./cmd/kritis/signer

# Copy the application binary into a thin base image
FROM gcr.io/distroless/base AS run-stage
WORKDIR /
COPY --from=build-stage /out/kritis-signer /kritis-signer
USER nonroot:nonroot
ENTRYPOINT ["/kritis-signer"]
