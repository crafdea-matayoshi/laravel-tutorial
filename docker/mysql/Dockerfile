FROM mysql:8-debian

WORKDIR .

RUN set -eux; \
        apt-get update; \
        apt-get install -y --no-install-recommends \
            vim \
        ; \
        rm -rf /var/lib/apt/lists/*
