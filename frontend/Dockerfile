FROM node:20-bullseye

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json pnpm-lock.yaml* ./
RUN corepack enable && corepack prepare pnpm@8.14.0 --activate && pnpm install

# Copy all source code
COPY . .

# Set environment variables
ENV VITE_PUBLIC_URL=https://DeduQuiz.onrender.com
ENV VITE_API_URL=https://DeduQuiz.onrender.com/api
ENV VITE_REDIS_URL=redis://red-d3q5oiripnbc73aarbsg:6379
ENV NODE_ENV=production
ENV PORT=10000

# Build frontend
RUN pnpm run build

# Expose port
EXPOSE 10000

# Start app using npx serve (no global install needed)
CMD ["npx", "serve", "-s", "build", "-l", "10000"]
