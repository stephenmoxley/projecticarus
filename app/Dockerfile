# Build Stage
FROM lacion/alpine-golang-buildimage:1.13 AS build-stage

LABEL app="build-projecticarus"
LABEL REPO="https://github.com/stephenmoxley/projecticarus"

ENV PROJPATH=/go/src/github.com/stephenmoxley/projecticarus

# Because of https://github.com/docker/docker/issues/14914
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

ADD . /go/src/github.com/stephenmoxley/projecticarus
WORKDIR /go/src/github.com/stephenmoxley/projecticarus

RUN make build-alpine

# Final Stage
FROM lacion/alpine-base-image:latest

ARG GIT_COMMIT
ARG VERSION
LABEL REPO="https://github.com/stephenmoxley/projecticarus"
LABEL GIT_COMMIT=$GIT_COMMIT
LABEL VERSION=$VERSION

# Because of https://github.com/docker/docker/issues/14914
ENV PATH=$PATH:/opt/projecticarus/bin

WORKDIR /opt/projecticarus/bin

COPY --from=build-stage /go/src/github.com/stephenmoxley/projecticarus/bin/projecticarus /opt/projecticarus/bin/
RUN chmod +x /opt/projecticarus/bin/projecticarus

# Create appuser
RUN adduser -D -g '' projecticarus
USER projecticarus

ENTRYPOINT ["/usr/bin/dumb-init", "--"]

CMD ["/opt/projecticarus/bin/projecticarus"]
