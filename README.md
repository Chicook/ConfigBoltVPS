# 🚀 Despliegue del Cliente y Servidor de ForoIA

Este proyecto incluye el cliente y el servidor para ForoIA. A continuación, encontrarás una guía para desplegar ambos contenedores utilizando Docker.

## 📌 Prerrequisitos

Asegúrate de tener Docker instalado en tu máquina. Para más información sobre cómo instalar Docker, visita la [documentación oficial de Docker](https://docs.docker.com/get-docker/).

## 🌐 Dockerfile del Cliente

Este es el Dockerfile para el cliente, ubicado en `src`:

```dockerfile
# Dockerfile para el Cliente
FROM node:18

# Crear directorio de trabajo
WORKDIR /app

# Copiar todo el proyecto en el contenedor
COPY . .

# Moverse al directorio src
WORKDIR /app/src

# Instalar dependencias del cliente
RUN npm install

# Construir el proyecto del cliente
RUN npm run build

# Exponer el puerto en el que escucha el cliente
EXPOSE 5173

# Comando de inicio para el cliente
CMD ["npm", "run", "dev"]
```

## 🌐 Dockerfile del Servidor

Este es el Dockerfile para el servidor, ubicado en `server`:

```dockerfile
# Dockerfile para el Servidor
FROM node:18

# Crear directorio de trabajo
WORKDIR /app

# Copiar archivos del servidor
COPY package.json package-lock.json ./
COPY server/ ./server

# Instalar dependencias del servidor
RUN npm install

# Exponer el puerto en el que escucha el servidor
EXPOSE 3000

# Comando de inicio para el servidor
CMD ["node", "server/index.js"]
```

## 🛠️ Comandos para Construir y Ejecutar los Contenedores

Para lanzar los contenedores del servidor y cliente, ejecuta los siguientes comandos (siendo foroia-server y client los nombres de los contenedores):

```bash
docker build -t foroia-server -f server/Dockerfile .
docker run -d -p 3000:3000 --name foroia-server foroia-server

docker build -t foroia-client -f src/Dockerfile .
docker run -d -p 5173:5173 --name foroia-client foroia-client
```

## 🗑️ Comandos para Detener y Eliminar los Contenedores

Si necesitas detener y eliminar los contenedores, ejecuta los siguientes comandos:

```bash
docker stop foroia-server
docker rm foroia-server

docker stop foroia-client
docker rm foroia-client
```

## ⚙️ Configuración de Vite

A continuación, se muestra el archivo de configuración de Vite para el cliente:

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    host: '0.0.0.0',  // Escucha en todas las interfaces de red
    port: 5173       // Asegúrate de que esté en el puerto 5173
  }
});
```

## 🚀 ¡Ponte en Marcha!

Con estos pasos, tu cliente y servidor de ForoIA estarán desplegados y listos para usarse. ¡Disfruta del desarrollo y no dudes en contribuir si encuentras mejoras! 😄
