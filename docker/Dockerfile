FROM alpine as Source

WORKDIR /

RUN apk update && \
    apk add git
RUN git clone --depth=1 https://github.com/sdu-concurrency/group-website /git

FROM ruby as Build
COPY --from=Source /git/web /srv/jekyll
RUN apt update && \
    apt install -y graphviz
WORKDIR /srv/jekyll
RUN bundle install
RUN bundle exec jekyll build --trace
RUN mv /srv/jekyll/_site /site

FROM jolielang/leonardo
ENV LEONARDO_WWW /web
COPY --from=Build /site $LEONARDO_WWW
