FROM node:20.12.2 AS builder
WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install


COPY . .
RUN npm run build


FROM node:20-alpine AS production
WORKDIR /usr/src/app


COPY package*.json ./
RUN npm install --only=production

COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/node_modules ./node_modules


EXPOSE 3000
CMD ["npm", "run", "start:prod"]