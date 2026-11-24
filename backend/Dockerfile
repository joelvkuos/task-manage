FROM node:22-alpine AS build
WORKDIR /app

COPY package.json package-lock.json .npmrc ./
COPY backend/package.json ./backend/
RUN npm ci --workspace backend

COPY backend/ ./backend/
RUN npm run build --workspace backend

FROM node:22-alpine
ENV NODE_ENV=production
WORKDIR /app

COPY package.json package-lock.json .npmrc ./
COPY backend/package.json ./backend/
RUN npm ci --workspace backend --omit=dev && npm cache clean --force

COPY --from=build /app/backend/dist ./backend/dist
COPY backend/src/db/schema.sql ./backend/src/db/schema.sql

WORKDIR /app/backend
USER node
EXPOSE 5000
CMD ["node", "dist/server.js"]
