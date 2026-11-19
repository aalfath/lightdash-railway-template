# Headless Chromium that Lightdash uses to render chart and dashboard images
# (scheduled deliveries, Slack unfurls, image exports). Private service, no public domain.
# Same version Lightdash pins upstream (docker/Dockerfile.headless-browser), pinned by digest.
FROM ghcr.io/browserless/chromium:v2.49.0@sha256:a0c024087a2625704ffe3644ea2d886324b57ab1cc9124fb15eacc6597429ca1

# Railway's private network is IPv6, so listen on :: (all addresses).
ENV HOST=:: \
    PORT=3000 \
    CONNECTION_TIMEOUT=180000 \
    MAX_CONCURRENT_SESSIONS=5
