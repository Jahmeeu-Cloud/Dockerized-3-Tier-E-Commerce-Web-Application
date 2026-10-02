# --- STAGE 1: Builder ---
FROM node:20.11.0-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# --- STAGE 2: Runtime ---
FROM node:20.11.0-alpine
WORKDIR /app

# Create non-root user (UID 1001)
RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -S appuser

# Copy production node_modules from builder
COPY --from=builder /app/node_modules ./node_modules
COPY . /app

# Set permissions and switch to non-root user
RUN chown -R appuser:appgroup /app
USER 1001

ENV NODE_ENV=production
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s CMD wget --no-verbose --tries=1 http://localhost:3000/health || exit 1

CMD ["node", "index.js"]
