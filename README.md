# 🧩 Product Catalog Microservices System

Sistema distribuido basado en Spring Boot + Spring Cloud + Eureka +
Vault + PostgreSQL.

------------------------------------------------------------------------
🏗️ Arquitectura basada en Microservicios

El sistema está compuesto por:
                    ┌────────────────────┐
                    │   Discovery Server │
                    │      (Eureka)      │
                    │       :8761        │
                    └─────────▲──────────┘
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
┌───────────────┐   ┌────────────────┐   ┌────────────────┐
│ product-service│   │ discount-service│   │ catalog-service│
│     :8081     │   │      :8082     │   │      :8083     │
└───────────────┘   └────────────────┘   └────────────────┘
         │                    │                    │
         └────────── PostgreSQL Database ──────────┘


# 📌 Arquitectura General

Microservicios:

-   discovery-service (Eureka Server) → 8761
-   product-service → 8081
-   discount-service → 8082
-   catalog-service → 8083

Comunicación mediante: - Eureka Service Discovery - RestClient - Spring
Cloud LoadBalancer

------------------------------------------------------------------------

# 🏗️ Arquitectura

Discovery Server registra todos los servicios. Catalog-service consume
product-service y discount-service.

------------------------------------------------------------------------

# 📦 Servicios

## discovery-service

Servidor Eureka. @EnableEurekaServer URL: http://localhost:8761

## product-service

GET /products GET /products/{id} POST /products PUT /products/{id}
DELETE /products/{id}

## discount-service

GET /discounts GET /discounts/{id} GET /discounts/product/{productId}
POST /discounts PUT /discounts/{id} DELETE /discounts/{id}

## catalog-service

GET /catalog/products/{id}

Calcula precio final aplicando descuento.

------------------------------------------------------------------------

# 🔐 Vault Config

spring.cloud.vault.host=localhost spring.cloud.vault.port=8200
spring.cloud.vault.authentication=TOKEN

------------------------------------------------------------------------

# 🗄️ Base de Datos

CREATE TABLE products ( id SERIAL PRIMARY KEY, codigo VARCHAR(50),
nombre VARCHAR(100), descripcion TEXT, precio NUMERIC(10,2), estado
VARCHAR(20), creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP );

CREATE TABLE discounts ( id SERIAL PRIMARY KEY, product_id INT,
porcentaje NUMERIC(5,2), estado VARCHAR(20), creado_en TIMESTAMP DEFAULT
CURRENT_TIMESTAMP );

------------------------------------------------------------------------

# 🚀 Ejecución

1.  Levantar PostgreSQL
2.  Ejecutar discovery-service
3.  Ejecutar product-service
4.  Ejecutar discount-service
5.  Ejecutar catalog-service

------------------------------------------------------------------------

# 📊 Tecnologías

Java 17 Spring Boot Spring Cloud Eureka Vault PostgreSQL Docker Maven

------------------------------------------------------------------------

Autor: Arquitectura de Microservicios
