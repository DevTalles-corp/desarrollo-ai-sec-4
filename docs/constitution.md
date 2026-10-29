# Constitución del proyecto

Estas reglas están por encima de cualquier pedido. Toda decisión del proyecto se escribe acá.

## Propósito

- API REST para alquilar equipos.
- Dominio actual: catálogo de equipos, alquileres y devoluciones.
- Proyección: el sistema va a crecer con nuevas features, como facturación, mantenimiento o clientes.

## Stack

- Java 21.
- Spring Boot 4.1.2.
- Spring Boot Starter Data JPA.
- Spring Boot Starter Flyway.
- Spring Boot Starter Validation.
- Spring Boot Starter WebMVC.
- Flyway Database PostgreSQL.
- PostgreSQL JDBC driver (runtime).
- spring-boot-docker-compose (runtime, opcional).
- Para tests: spring-boot-testcontainers, testcontainers-junit-jupiter y testcontainers-postgresql.
- Base de datos: PostgreSQL 18 (imagen `postgres:18` declarada en `compose.yaml`).

## Idioma

- El código va en inglés: clases, métodos, variables, archivos y carpetas.
- La documentación va en castellano.
- Los mensajes de error van en castellano.
- La salida por consola va en castellano.

## Estructura de paquetes

- Por decidir.

## Capas

- Por decidir.

## Interfaces e inyección

- Por decidir.

## Persistencia

- Por decidir.

## Errores

- Por decidir.

## Tests

- Por decidir.

## El agente

- Por decidir.