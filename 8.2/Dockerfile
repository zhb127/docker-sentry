FROM python:2.7-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        gcc \
        git \
        libffi-dev \
        libpq-dev \
        libxml2-dev \
        libxslt-dev \
        libyaml-dev \
    && rm -rf /var/lib/apt/lists/*

# Sane defaults for pip
ENV PIP_NO_CACHE_DIR off
ENV PIP_DISABLE_PIP_VERSION_CHECK on

ENV SENTRY_VERSION 8.2.3
ENV SENTRY_DOWNLOAD_SHA256 9e6efa28b7c2d94d18d3217887d99ee0fc9772aadff422965aa6fd29604c32ca

RUN pip install sentry==$SENTRY_VERSION --hash=sha256:$SENTRY_DOWNLOAD_SHA256

ENV SENTRY_CONF=/etc/sentry \
    SENTRY_FILESTORE_DIR=/var/lib/sentry/files

RUN mkdir -p $SENTRY_CONF && mkdir -p $SENTRY_FILESTORE_DIR

COPY sentry.conf.py /etc/sentry/
COPY config.yml /etc/sentry/

# Allow Celery to run as root
ENV C_FORCE_ROOT 1

COPY docker-entrypoint.sh /

EXPOSE 9000
VOLUME /var/lib/sentry/files

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["start"]
