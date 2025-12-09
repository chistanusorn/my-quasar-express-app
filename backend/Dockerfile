# Stage 1: Build
FROM node:20-alpine AS builder


WORKDIR /app


COPY package*.json ./
RUN npm ci


COPY . .


# Stage 2: Production
FROM node:20-alpine

WORKDIR /app

# Create logs directory with correct permissions
RUN mkdir -p logs && chown -R node:node logs

# Copy built artifacts from builder stage with correct ownership
COPY --from=builder --chown=node:node /app /app

# Switch to non-root user
USER node

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:3000/api/demo || exit 1

EXPOSE 3000

CMD ["node", "server.js"]
