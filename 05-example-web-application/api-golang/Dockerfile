FROM golang:1.19-buster as build

WORKDIR /app

RUN useradd -u 1001 appuser

COPY . .

RUN go mod download

RUN go build \
  -ldflags="-linkmode external -extldflags -static" \
  -tags netgo \
  -o api-golang

FROM scratch

ENV GIN_MODE=release

COPY --from=build /etc/passwd /etc/passwd

COPY --from=build /app/api-golang api-golang

USER appuser

EXPOSE 8080

CMD [ "/api-golang" ]