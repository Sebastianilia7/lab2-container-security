# Stage 1: Build
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY src/ ./src

# Stage 2: Production
FROM node:22-alpine AS production
WORKDIR /app

# Skapa non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Kopiera ALLT från builder istället för bara node_modules
COPY --from=builder /app /app

USER appuser
CMD ["node", "src/index.js"]
