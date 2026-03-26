# Gestión de Usuarios con FastAPI, PostgreSQL, Prometheus y Grafana

## Objetivo del Taller
El propósito de este Taller es experimentar con la contenerización de aplicaciones mediante Docker, construyendo un sistema CRUD para gestionar usuarios, e integrando herramientas de monitoreo y visualización de métricas.

## Justificación y Arquitectura
Este sistema se construyó utilizando una arquitectura de microservicios contenerizados:
* **FastAPI:** Elegido por su velocidad y facilidad para crear APIs RESTful con Python.
* **PostgreSQL:** Actúa como la base de datos relacional para persistir los datos de los usuarios.
* **Prometheus:** Implementado para el monitoreo de la API, recolectando métricas en tiempo real (como la cantidad de usuarios creados).
* **Grafana:** Utilizado para conectar con Prometheus y crear un Dashboard visual de las métricas.
* **Docker:** Se utilizó una red interna (`red-taller`) para aislar los servicios y permitir que se comuniquen entre sí de forma segura. Se creó un `Dockerfile` personalizado para empaquetar la aplicación de FastAPI.

## Pasos para ejecutar el proyecto

1. Crear la red de Docker:
   `docker network create red-taller`
2. Levantar la base de datos PostgreSQL:
   `docker run -d --name postgres --network red-taller -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=mydb -p 5432:5432 postgres:13`
3. Construir la imagen de la API:
   `docker build -t fastapi-app ./app`
4. Ejecutar la API:
   `docker run -d --name api --network red-taller -p 8000:8000 fastapi-app`
5. Levantar Prometheus (Monitoreo):
   `docker run -d --name prometheus -p 9090:9090 -v ${PWD}/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus`
6. Levantar Grafana (Dashboard):
   `docker run -d --name grafana -p 3000:3000 grafana/grafana`