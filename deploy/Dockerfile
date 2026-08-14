# SB branded Portolan browser for data.studiobereikbaar.nl
#
# Build context is the REPO ROOT (Coolify runs compose with
# --project-directory=<repo root>), so every path here is repo-relative.

FROM node:22-alpine AS build
WORKDIR /app

# Deps first so a source-only change reuses the layer.
COPY package.json package-lock.json ./
RUN npm ci --no-audit --no-fund

COPY . .
# `build:minimal` = vite --mode minimal, which turns sourcemaps off
# (vite.config.js: `sourcemap: mode !== "minimal"`). Shipping the default build
# publishes 534 .map files and 49 MB of readable source; minimal is 17 MB.
RUN npm run build:minimal

FROM caddy:2.10-alpine
COPY --from=build /app/dist /srv
COPY deploy/Caddyfile /etc/caddy/Caddyfile
EXPOSE 8080
