FROM ruby:2.5.1-alpine

RUN apk add --update --no-cache \
    build-base \
    libxml2-dev \
    libxslt-dev \
    shared-mime-info \
    postgresql-dev \
    nodejs

WORKDIR /xpaste
COPY . .

RUN bundle config build.nokogiri --use-system-libraries && \
    bundle install --clean --no-cache --without development

CMD /xpaste/entrypoint.sh
