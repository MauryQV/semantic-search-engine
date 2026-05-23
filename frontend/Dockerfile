# --- Etapa 1: Construcción ---
FROM node:18-alpine AS build

WORKDIR /app

# Copiar los archivos de configuración y dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm install

# Copiar el resto del código
COPY . .

# Compilar la aplicación de React (Vite crea la carpeta 'dist')
RUN npm run build

# --- Etapa 2: Servidor Web (Nginx) ---
FROM nginx:alpine

# Copiar los archivos compilados de la etapa anterior a la carpeta de Nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Ajustar Nginx para que escuche en el puerto 8080 (el que usa Cloud Run)
RUN sed -i 's/listen  (.*)80;/listen 8080;/' /etc/nginx/conf.d/default.conf

# Exponer el puerto
EXPOSE 8080

# Iniciar Nginx
CMD ["nginx", "-g", "daemon off;"]