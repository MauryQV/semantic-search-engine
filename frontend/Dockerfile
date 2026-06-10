# --- Etapa 1: Compilación de la App (Usando Node 20) ---
FROM node:20-alpine AS build
WORKDIR /app

# Copiar archivos de dependencias
COPY package*.json ./
RUN npm install

# Copiar el resto del código del frontend
COPY . .

# Compilar el proyecto (Vite generará la carpeta /app/dist)
RUN npm run build


# --- Etapa 2: Servidor de Producción (Nginx) ---
FROM nginx:alpine

# Escribimos la configuración para forzar el puerto 8080 y evitar errores 404 en React
RUN echo 'server { \
    listen 8080; \
    location / { \
    root /usr/share/nginx/html; \
    index index.html index.htm; \
    try_files $uri $uri/ /index.html; \
    } \
    }' > /etc/nginx/conf.d/default.conf

# Copiar los archivos compilados desde la etapa de Node
COPY --from=build /app/dist /usr/share/nginx/html

# Informar a Cloud Run del puerto
EXPOSE 8080

# Arrancar Nginx
CMD ["nginx", "-g", "daemon off;"]