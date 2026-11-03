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
- El `controller` recibe y devuelve DTOs; la entidad no sale del `service`. Ejemplo: `EquipmentController.createEquipment(...)` recibe un `EquipmentRequest` y devuelve un `EquipmentResponse`.
- El `service` recibe y devuelve los mismos DTOs que el `controller`: no hay DTOs propios del `service`. Ejemplo: `EquipmentService.createEquipment(...)` recibe un `EquipmentRequest` y devuelve un `EquipmentResponse`.
- Los DTOs son `record`; la entidad es una clase. Ejemplo: `EquipmentRequest` y `EquipmentResponse` son records, y `Equipment` es una clase JPA.
- El mapeo de entidad a DTO y de DTO a entidad vive en una clase mapper por feature, en su subpaquete `mapper`, y la usa el `service`. Ejemplo: `EquipmentService` usa `EquipmentMapper`.
- El mapper solo convierte datos: la lógica de negocio queda en el `service`. Ejemplo: el estado inicial de un equipo lo decide `EquipmentService`, no `EquipmentMapper`.
- El formato de la entrada se valida en el DTO con Bean Validation, y el `controller` marca el request con `@Valid`: si no cumple, responde 400. Ejemplo: `@NotBlank` en el nombre del equipo.

## Interfaces e inyección

- Dentro de una feature, las clases dependen de clases concretas. Ejemplo: `EquipmentController` recibe `EquipmentService`.
- El repository es la excepción: es una interfaz, porque Spring Data JPA genera su implementación. Ejemplo: `EquipmentRepository` extiende `JpaRepository`.
- El mapper es un bean con `@Component` y una clase concreta. Ejemplo: `EquipmentMapper` lleva `@Component`.
- Cuando una feature depende de otra, usa una interfaz por rol, con solo los métodos que necesita, en el subpaquete `api` de la otra feature. Ejemplo: `rental` recibe `catalog.api.CatalogApi`.
- Dentro de una feature, una interfaz aparece solo cuando la pide un patrón, como Strategy. Ejemplo: `PricingStrategy`, con `DailyPricing` para la tarifa por día y `WeeklyPricing` para la tarifa por semana.
- Cuando una interfaz tiene varias implementaciones, cada una lleva `@Component` con un nombre, y el service las recibe todas en un `Map<String, Interfaz>` cuya clave es el nombre del bean. El service elige la implementación según el pedido. Ejemplo: `RentalService` recibe `Map<String, PricingStrategy>` y usa `"weekly"` para un alquiler por semana.
- Las dependencias se inyectan por constructor, con campos `final`. Con un solo constructor no hace falta `@Autowired`. Ejemplo: `EquipmentService` recibe `EquipmentRepository` y `EquipmentMapper` en su constructor.

## Persistencia

- Por decidir.

## Errores

- Por decidir.

## Tests

- Por decidir.

## El agente

- Por decidir.