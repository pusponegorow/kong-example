# STAGE-1
# Build the kong plugin
FROM golang:1.19-alpine3.15 AS plugin-builder

WORKDIR /builder

COPY . .

RUN apk add make
RUN make build

# STAGE-2
# Build kong including the plugin that already build in previous stage
FROM kong:3.1.0-alpine

COPY --from=plugin-builder /builder/header-validation /kong/go-plugins/header-validation

USER kong
ENTRYPOINT ["/docker-entrypoint.sh", "kong", "docker-start"]

EXPOSE 8000
EXPOSE 8001
EXPOSE 8443
EXPOSE 8444

STOPSIGNAL SIGQUIT
HEALTHCHECK --interval=10s --timeout=10s --retries=10 CMD kong health