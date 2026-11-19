# Lightdash scheduler worker (private service): scheduled deliveries, CSV/image exports,
# validation and other background jobs. Same image and version as the web service.
FROM lightdash/lightdash:3.341.0@sha256:8242b5ff0f9522f6c31c0b63184325d4643be32ee7b6d32599716d1f2686f49a

ENV PORT=8080 \
    SECURE_COOKIES=true \
    TRUST_PROXY=true \
    SCHEDULER_ENABLED=false \
    SCHEDULER_CONCURRENCY=3 \
    LIGHTDASH_INSTALL_TYPE=docker_image \
    LIGHTDASH_MAX_PAYLOAD=40mb \
    HEADLESS_BROWSER_PORT=3000

# Health endpoint: GET /api/v1/health on PORT (worker state only, no login).
ENTRYPOINT ["dumb-init", "--", "/usr/bin/prod-entrypoint.sh"]
CMD ["node", "dist/scheduler.js"]
