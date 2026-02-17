# 1. Build stage
FROM node:22-alpine AS builder

WORKDIR /app

# Install dependencies
COPY package.json package-lock.json ./

RUN npm install

# Copy source code
COPY . .

# Build the application
RUN npm run build

# 2. Run stage
FROM node:22-alpine

WORKDIR /app

# set production environment
ENV NODE_ENV=production

# copy only necessary files from builder stage
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./

# Install only production dependencies
RUN npm install --omit=dev

# Expose the port the app runs on
EXPOSE 3000

# Start the application
CMD ["npm", "start"]