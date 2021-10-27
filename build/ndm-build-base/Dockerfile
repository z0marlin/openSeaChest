FROM debian:buster as build
RUN apt-get update && apt-get install -y make git gcc
WORKDIR /tmp/openSeachest
COPY . .
WORKDIR /tmp/openSeachest/Make/gcc
RUN make release
WORKDIR /tmp/openSeachest
RUN cp opensea-common/Make/gcc/lib/libopensea-common.a /usr/lib && \
    cp opensea-operations/Make/gcc/lib/libopensea-operations.a /usr/lib && \
    cp opensea-transport/Make/gcc/lib/libopensea-transport.a /usr/lib

FROM golang:1.14.7
RUN apt-get update && apt-get install -y libudev-dev libblkid-dev
COPY --from=build /usr/lib/libopensea-* /usr/lib/
WORKDIR /go/src/github.com/openebs/node-disk-manager/
ARG DBUILD_DATE
ARG DBUILD_REPO_URL="https://github.com/openebs/openSeaChest"
ARG DBUILD_SITE_URL="http://www.openebs.io/"

LABEL org.label-schema.schema-version="1.0"
LABEL org.label-schema.name="ndm-base-build"
LABEL org.label-schema.description="Base image for building ndm binaries"
LABEL org.label-schema.build-date=$DBUILD_DATE
LABEL org.label-schema.vcs-url=$DBUILD_REPO_URL
LABEL org.label-schema.url=$DBUILD_SITE_URL
