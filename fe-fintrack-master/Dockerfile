# ======== STAGE 1: Build Vite App ========
FROM node:18-alpine AS build

WORKDIR /app

# ✅ Cài vite global để tránh lỗi "vite: not found"
RUN npm install -g vite

# Copy package files và cài dependency (bao gồm cả devDependencies)
COPY package*.json ./
RUN npm install

# Copy toàn bộ source code
COPY . .

# ✅ Thiết lập biến môi trường build
ARG VITE_API_BASE_URL
ENV VITE_API_BASE_URL=$VITE_API_BASE_URL

# ✅ Build trực tiếp bằng vite (không dùng npx nữa)
RUN vite build

# ======== STAGE 2: Serve with Nginx ========
FROM nginx:alpine

# Xóa default config
RUN rm /etc/nginx/conf.d/default.conf

# Copy nginx.conf của bạn
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy kết quả build từ stage 1
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]