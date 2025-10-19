# Base image
FROM node:19-bullseye

# Set working directory
WORKDIR /app

# Copy package files and lock file
COPY package*.json pnpm-lock.yaml* ./

# Enable corepack and install pnpm
RUN corepack enable && corepack prepare pnpm@8.14.0 --activate && pnpm install

# Copy all source code
COPY . .

# Build-time environment variables for frontend
ENV VITE_PUBLIC_URL=https://your-app-name.onrender.com
ENV VITE_API_URL=https://your-app-name.onrender.com/api
ENV VITE_REDIS_URL=redis://red-d3q5oiripnbc73aarbsg:6379
ENV NODE_ENV=production
ENV PORT=10000

# Build frontend
RUN pnpm run build

# Expose port
EXPOSE 10000

# Start command
CMD ["pnpm", "run", "run:prod"]
