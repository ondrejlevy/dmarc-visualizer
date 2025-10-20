FROM python:3.13-alpine3.22

# Instalace system závislostí
RUN apk add --update --no-cache \
    libxml2-dev \
    libxslt-dev \
    curl

# Instalace parsedmarc s build závislostmi
RUN apk add --update --no-cache --virtual .build_deps \
    build-base \
    libffi-dev \
    && pip install --no-cache-dir parsedmarc==6.18.7 \
    && apk del .build_deps

# Vytvoření non-root uživatele pro bezpečnost
RUN adduser -D -s /bin/sh parsedmarc

# Kopírování konfigurace
COPY parsedmarc.ini /
#COPY GeoLite2-Country.mmdb /usr/share/GeoIP/GeoLite2-Country.mmdb

# Přepnutí na non-root uživatele
USER parsedmarc

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import parsedmarc; print('OK')" || exit 1
