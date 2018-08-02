FROM alpine:latest

RUN apk add --no-cache \
    bash \
    curl

COPY ./periodic /etc/periodic

ENTRYPOINT ["crond"]

HEALTHCHECK --interval=5s --timeout=2s \
    CMD ps | grep '[c]rond' || exit 1

# Start cron in foreground with logging to stderr
CMD ["-f", "-d8"]
