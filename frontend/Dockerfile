# Base image: Node.js 20 LTS
FROM node:20-bullseye

# Set working directory
WORKDIR /app

# Copy package files and lock file
COPY package*.json pnpm-lock.yaml* ./

# Enable corepack and install pnpm
RUN corepack enable && corepack prepare pnpm@8.14.0 --activate && pnpm install

# Copy all source code
COPY . .

# Build-time environment variables for frontend
ENV VITE_PUBLIC_URL=https://DeduQuiz.onrender.com
ENV VITE_API_URL=https://DeduQuiz.onrender.com/api
ENV VITE_REDIS_URL=redis://red-d3q5oiripnbc73aarbsg:6379
ENV NODE_ENV=production
ENV PORT=10000

# Build frontend
RUN pnpm run build

# Install 'serve' to serve the frontend build folder
RUN pnpm install -g serve

# Expose port
EXPOSE 10000

# Correct start command to serve build folder
CMD ["serve", "-s", "build", "-l", "10000"]
