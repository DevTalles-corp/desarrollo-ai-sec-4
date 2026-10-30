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

- Un paquete por feature dentro de `com.devtalles.rental`, porque el sistema va a crecer. Ejemplo: `catalog` para el catálogo de equipos y `rental` para los alquileres.
- Dentro de cada feature, un subpaquete por responsabilidad. Ejemplo: `catalog.model.Equipment` y `catalog.mapper.EquipmentMapper`.
- Las responsabilidades posibles son: `api`, `controller`, `dto`, `mapper`, `model`, `repository` y `service`.
- Una feature usa de otra solo su subpaquete `api`. Ejemplo: `rental` usa `catalog.api.CatalogApi`, nunca `catalog.repository.EquipmentRepository`.
- El subpaquete `api` aparece recién cuando otra feature lo necesita.
- Lo transversal, lo que no es de ninguna feature, vive en `shared`, con un subpaquete por tema. Ejemplo: `shared.error` para el manejo de errores, `shared.config` para la configuración de Spring y `shared.security` para la seguridad. Una utilidad va en el subpaquete del tema al que sirve.
- Nada transversal va al mismo nivel que las features.

## Capas

- Cada feature tiene tres capas cerradas: `controller`, `service` y `repository`. Ejemplo: `EquipmentController` depende de `EquipmentService`, y solo `EquipmentService` depende de `EquipmentRepository`.
- El `controller` nunca accede al `repository`, aunque la operación sea una simple lectura. Ejemplo: el endpoint que lista equipos delega en `EquipmentService.listEquipments()` en lugar de llamar directamente a `EquipmentRepository.findAll()`.
- El `controller` recibe el pedido HTTP y responde con el código de estado. Ejemplo: `EquipmentController.createEquipment(...)` delega el alta en `EquipmentService` y responde 201 con lo que devuelve el service.
- El `service` contiene las reglas de negocio y coordina la transacción. Ejemplo: `EquipmentService.createEquipment(...)` verifica que no exista otro equipo con el mismo serial antes de guardarlo.
- El `repository` se encarga exclusivamente del acceso a datos. Ejemplo: `EquipmentRepository.findBySerial(...)` ejecuta la consulta a PostgreSQL.
- La anotación `@Transactional` vive en el `service`, nunca en el `controller` ni en el `repository`. Ejemplo: `EquipmentService.createEquipment(...)` es el método anotado, no el endpoint ni el repository.

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