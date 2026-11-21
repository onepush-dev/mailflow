# MailFlow backend on Railway: the official ghcr.io/maathimself/mailflow-backend:4.5.1 image (AGPL-3.0), code unmodified.
# Railway layer: only defaults baked in. Postgres is the bundled `postgres` service (user/db `postgres`),
# and TRUST_PROXY_HOPS=2 counts Railway's edge plus MailFlow's Caddy frontend.
FROM ghcr.io/maathimself/mailflow-backend:4.5.1
LABEL org.opencontainers.image.source="https://github.com/onepush-dev/mailflow"
ENV NODE_ENV=production \
    PORT=3000 \
    TRUST_PROXY_HOPS=2 \
    DB_PORT=5432 \
    DB_NAME=postgres \
    DB_USER=postgres \
    IMAGE_CACHE_DIR=/var/cache/mailflow/images \
    IMAGE_CACHE_MAX_MB=512
EXPOSE 3000
