# Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
COPY vite.config.js ./
RUN npm ci
COPY . .
RUN npm run build

# Serve static build via Nginx
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
# Nginx reverse proxy to backend service name in k8s cluster
RUN sed -i 's#location / {#location / { try_files $uri $uri/ /index.html; }#' /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
