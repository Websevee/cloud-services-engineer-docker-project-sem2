FROM node:16-alpine AS build
WORKDIR /app
ENV NODE_ENV=dev
ENV VUE_APP_API_URL=http://localhost:8081
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]