# 🏗️ Microservices Architecture - Spring Boot + Eureka + Vault + PostgreSQL

------------------------------------------------------------------------

# 📌 Descripción General

Este proyecto implementa una arquitectura basada en microservicios
utilizando:

-   Spring Boot 4
-   Spring Cloud Netflix Eureka
-   Spring Cloud Config
-   HashiCorp Vault
-   PostgreSQL
-   Spring Cloud LoadBalancer
-   RestClient (Spring 6+)

El sistema está compuesto por múltiples servicios desacoplados que se
registran dinámicamente en Eureka y se comunican entre sí usando
descubrimiento de servicios.

------------------------------------------------------------------------

# 🧱 Arquitectura General

                     ┌───────────────────────────┐
                     │   Discovery Server        │
                     │        (Eureka)           │
                     │           :8761           │
                     └─────────────┬─────────────┘
                                   │
            ┌──────────────────────┼──────────────────────┐
            │                      │                      │
    ┌───────┴────────┐    ┌────────┴────────┐    ┌────────┴────────┐
    │ product-service │    │ discount-service│    │ catalog-service  │
    │     :8081       │    │     :8082       │    │     :8083        │
    └───────┬────────┘    └────────┬────────┘    └────────┬────────┘
            │                      │                      │
            └──────────────┬───────┴──────────┬───────────┘
                           │                  │
                     ┌─────┴──────────────────┴─────┐
                     │        PostgreSQL Database     │
                     └───────────────────────────────┘

------------------------------------------------------------------------

# 🚀 Servicios

## 1️⃣ Discovery Service (Eureka Server)

Puerto: 8761

Responsable de registrar y descubrir microservicios.

### Dependencias principales:

-   spring-cloud-starter-netflix-eureka-server

### Activación:

``` java
@EnableEurekaServer
```

------------------------------------------------------------------------

## 2️⃣ Product Service

Puerto: 8081

Responsable de gestionar productos.

### Funcionalidades:

-   CRUD de productos
-   Persistencia en PostgreSQL

### Endpoints:

GET /products\
GET /products/{id}\
POST /products\
PUT /products/{id}\
DELETE /products/{id}

------------------------------------------------------------------------

## 3️⃣ Discount Service

Puerto: 8082

Responsable de gestionar descuentos por producto.

### Endpoints:

GET /discounts/product/{productId}

------------------------------------------------------------------------

## 4️⃣ Catalog Service

Puerto: 8083

Servicio agregador que consulta:

-   product-service
-   discount-service

Calcula precio final con descuento aplicado.

### Endpoint principal:

GET /catalog/{productId}

------------------------------------------------------------------------

# 🗄️ Base de Datos

Motor: PostgreSQL

Cada microservicio puede tener su propia base de datos (arquitectura
recomendada) o compartir esquema.

------------------------------------------------------------------------

# 🔐 Integración con Vault

Vault almacena credenciales sensibles:

-   db.url
-   db.username
-   db.password

Configuración en application.properties:

    spring.config.import=optional:configserver:http://localhost:8888,optional:vault://
    spring.cloud.vault.host=localhost
    spring.cloud.vault.port=8200
    spring.cloud.vault.authentication=TOKEN
    spring.cloud.vault.token=root
    spring.cloud.vault.kv.enabled=true
    spring.cloud.vault.kv.backend=secret
    spring.cloud.vault.kv.application-name=db

------------------------------------------------------------------------

# 🔄 Flujo de Comunicación

1.  Microservicio arranca
2.  Se registra en Eureka
3.  Otros servicios lo descubren dinámicamente
4.  Catalog usa RestClient con LoadBalancer
5.  LoadBalancer resuelve instancia desde Eureka

------------------------------------------------------------------------

# ▶️ Orden de Ejecución

1.  Levantar PostgreSQL
2.  Levantar Vault
3.  Levantar Config Server (si aplica)
4.  Levantar Discovery Service
5.  Levantar product-service
6.  Levantar discount-service
7.  Levantar catalog-service

------------------------------------------------------------------------

# 🧩 Tecnologías Utilizadas

-   Java 17+
-   Spring Boot 4
-   Spring Cloud 2026
-   Hibernate 7
-   HikariCP
-   Maven

------------------------------------------------------------------------

# 📦 Cómo ejecutar

Desde cada microservicio:

    mvn clean install
    mvn spring-boot:run

O desde IDE ejecutando la clase Application.

------------------------------------------------------------------------

# 📊 Beneficios de la Arquitectura

✔ Escalabilidad independiente\
✔ Despliegue independiente\
✔ Desacoplamiento\
✔ Tolerancia a fallos\
✔ Seguridad centralizada

------------------------------------------------------------------------

# 👨‍💻 Autor

Proyecto académico/profesional basado en arquitectura moderna de
microservicios.

------------------------------------------------------------------------
