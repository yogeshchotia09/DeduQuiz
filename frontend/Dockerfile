# SPDX-FileCopyrightText: 2023 Marlon W (Mawoka)
#
# SPDX-License-Identifier: MPL-2.0

### Build Step
# pull the Node.js Docker image
FROM node:19-bullseye as builder
# ENV API_URL=http://api:80
ENV API_URL=https://mawoka.eu
#Ah, I've noticed that!!!
ENV REDIS_URL=redis://localhost:6379
# This Mapbox-token is restricted to the following urls: classquiz.de, classquiz.mawoka.eu, test.com
ENV VITE_HCAPTCHA=ee81b2a1-acf3-4d20-b2a4-a7ea94c7eba5
# Hacaptcha secret ES_e0f8139cb5ad467d892e8d73a020cd2b
# ENV VITE_SENTRY=https://75cb4ef1be624d8f81bbaf864b722f8a@glitch.mawoka.eu/2
#ENV VITE_GOOGLE_AUTH_ENABLED=true
#ENV VITE_GITHUB_AUTH_ENABLED=true
ENV VITE_CAPTCHA_ENABLED=false
#ENV VITE_REGISTRATION_DISABLED=True
#ENV VITE_PLAUSIBLE_DATA_URL=
# change working directory
WORKDIR /usr/src/app


# copy the package.json files from local machine to the workdir in container
COPY package*.json ./
COPY pnpm-lock.yaml ./

# run npm install in our local machine
RUN corepack enable && corepack prepare pnpm@8.14.0 --activate && pnpm i

# copy the generated modules and all other files to the container
COPY . .

# build the application
FROM node:18

WORKDIR /app

# Copy package files
COPY package*.json pnpm-lock.yaml* ./

# Install pnpm
RUN npm install -g pnpm

# Install dependencies
RUN pnpm install

# Copy source code
COPY . .

# Build-time env vars for frontend
ENV VITE_PUBLIC_URL=https://your-app-name.onrender.com
ENV VITE_API_URL=https://your-app-name.onrender.com/api
ENV VITE_REDIS_URL=redis://red-d3q5oiripnbc73aarbsg:6379

# Build frontend
RUN pnpm run build

# Set production env
ENV NODE_ENV=production
ENV PORT=10000

EXPOSE 10000

CMD ["pnpm", "start"]

### Serve Step
# pull the Node.js Docker image
FROM node:19-bullseye-slim

# change working directory
WORKDIR /app
COPY --from=builder /usr/src/app/package.json .
COPY --from=builder /usr/src/app/pnpm-lock.yaml .
RUN corepack enable && corepack prepare pnpm@8.14.0 --activate && pnpm i
# copy files from previous step
COPY --from=builder /usr/src/app/build .
COPY --from=builder /usr/src/app/node_modules ./node_modules

# our app is running on port 3000 within the container, so need to expose it
EXPOSE 3000

# the command that starts our app
CMD ["pnpm", "run", "run:prod"]
