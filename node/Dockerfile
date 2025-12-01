# Build stage
ARG NODE_VERSION=20
FROM node:${NODE_VERSION}-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build the application (if needed)
RUN npm run build

# Production stage
ARG NODE_VERSION=20
FROM node:${NODE_VERSION}-alpine

WORKDIR /app

# Copy package files and install production dependencies only
COPY package*.json ./
RUN npm ci --only=production

# Copy built files from builder stage
COPY --from=builder /app/dist ./dist

# Run as non-root user
USER node

EXPOSE 3000

CMD ["node", "dist/index.js"]
