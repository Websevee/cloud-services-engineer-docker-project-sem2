# 1. Builder
FROM node:16-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ARG NODE_ENV=development
ARG VUE_APP_API_URL=/api

ENV NODE_ENV=${NODE_ENV}
ENV VUE_APP_API_URL=${VUE_APP_API_URL}

RUN npm run build


# 2. Runner
FROM nginx:1.29.1-alpine

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]