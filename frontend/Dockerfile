# MailFlow frontend on Railway: the official ghcr.io/maathimself/mailflow-frontend:4.5.1 image (AGPL-3.0), code unmodified.
# Railway terminates TLS, so Caddy runs with TLS_MODE=off (HTTP on :80) and proxies the API to the
# `backend` service over Railway's private network (BACKEND_HOST is a reference variable).
FROM ghcr.io/maathimself/mailflow-frontend:4.5.1
LABEL org.opencontainers.image.source="https://github.com/onepush-dev/mailflow"
ENV TLS_MODE=off \
    BACKEND_HOST=backend.railway.internal \
    BACKEND_PORT=3000
EXPOSE 80
