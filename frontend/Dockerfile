# 1. Builder
FROM node:24-alpine AS build

ARG NODE_ENV=development
ARG VUE_APP_API_URL=/api
ARG APP_PATH

ENV NODE_ENV=${NODE_ENV}
ENV VUE_APP_API_URL=${VUE_APP_API_URL}
ENV APP_PATH=${APP_PATH}

WORKDIR /app

COPY package*.json ./
RUN npm ci --include=dev

COPY . .

# Для решения проблемы совместимости
ENV NODE_OPTIONS="--openssl-legacy-provider"

RUN npm run build


# 2. Runner
FROM nginx:1.29.1-alpine

ARG APP_PATH

RUN addgroup -S appgroup && adduser -S appuser -G appgroup


COPY nginx.conf.template /etc/nginx/templates/default.conf.template
COPY --from=build --chown=appuser:appgroup /app/dist /usr/share/nginx/html${APP_PATH}


# Настраиваем nginx для работы от non-root пользователя
RUN chown -R appuser:appgroup /var/cache/nginx && \
    chown -R appuser:appgroup /var/log/nginx && \
    chown -R appuser:appgroup /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R appuser:appgroup /var/run/nginx.pid

USER appuser

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]