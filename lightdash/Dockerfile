# Lightdash web app + API (public service).
# Official image, pinned by version and digest. Bump both together.
FROM lightdash/lightdash:3.341.0@sha256:8242b5ff0f9522f6c31c0b63184325d4643be32ee7b6d32599716d1f2686f49a

# Fixed settings for Railway (HTTPS behind Railway's proxy, scheduler runs as its own service).
# Every value can be overridden with a Railway variable of the same name.
ENV PORT=8080 \
    SECURE_COOKIES=true \
    TRUST_PROXY=true \
    SCHEDULER_ENABLED=false \
    LIGHTDASH_INSTALL_TYPE=docker_image \
    LIGHTDASH_MAX_PAYLOAD=40mb \
    HEADLESS_BROWSER_PORT=3000

EXPOSE 8080

# The image's entrypoint runs the database migrations (with a lock), then starts the server.
ENTRYPOINT ["dumb-init", "--", "/usr/bin/prod-entrypoint.sh"]
CMD ["node", "dist/index.js"]
