# Stage 1: Builder
FROM node:22.16-alpine AS builder
WORKDIR /app

COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm --no-audit --no-fund && \
    pnpm install --frozen-lockfile --prod=false

COPY . .
RUN pnpm build

# Stage 2: Caddy - Smaller than nginx with SPA support
FROM caddy:2.10-alpine

# Copy built React app
COPY --from=builder /app/dist /usr/share/caddy

# Copy Caddyfile configuration
COPY docker/Caddyfile /etc/caddy/Caddyfile

# Expose port
EXPOSE 80

# Caddy runs as non-root by default
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile"]