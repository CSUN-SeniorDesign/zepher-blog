FROM alpine:3.5 as build

ENV HUGO_VERSION 0.49.1
ENV HUGO_BINARY hugo_${HUGO_VERSION}_Linux-64bit.tar.gz

# Install Hugo
RUN set -x && \
  apk add --update wget ca-certificates && \
  wget https://github.com/spf13/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY} && \
  tar xzf ${HUGO_BINARY} && \
  rm -r ${HUGO_BINARY} && \
  mv hugo /usr/bin && \
  apk del wget ca-certificates && \
  rm /var/cache/apk/*

#circleCI grab should be here
COPY ./zephyrblog /site/

WORKDIR /site

RUN /usr/bin/hugo

FROM nginx:alpine

COPY ./conf/default.conf /etc/nginx/conf.d/default.conf

COPY --from=build /site/public /var/www/site

WORKDIR /var/www/site
