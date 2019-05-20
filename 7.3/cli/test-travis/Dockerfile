FROM debian:stretch-slim

# Get CURL for testing
RUN set -xe; \
    \
    apt-get update; \
    \
    apt-get install -y --no-install-recommends \
        curl \
    ; \
    apt-get clean; \
    \
    rm -rf /var/lib/apt/lists/*;

COPY test-travis-thunder-php /usr/bin/test-travis-thunder-php

# Set executable
RUN set -xe; \
    \
    chmod +x /usr/bin/test-travis-thunder-php;

ENV THUNDER_HOST="thunder-php"

CMD ["test-travis-thunder-php"]
