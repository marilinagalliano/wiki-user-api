# User API - API RESTful para la gestión de usuarios

Este proyecto es una API RESTful para la gestión de usuarios, construida utilizando Java, Spring Boot y basada en la arquitectura hexagonal (Ports and Adapters).
Incluye funcionalidades CRUD completas y documentación OpenAPI (Swagger).

---

## Tabla de Contenidos

* [Tecnologías](#tecnologías)
* [Estructura del Proyecto](#estructura-del-proyecto)
* [Endpoints Documentados](#endpoints-documentados)
* [Documentación OpenAPI](#documentación-openapi)
* [Principios SOLID Aplicados](#principios-solid-aplicados)
* [Capturas de Swagger UI](#capturas-de-swagger-ui)

---

## Tecnologías

| Tecnología        | Descripción                           |
| ----------------- | ------------------------------------  |
| Java              | Lenguaje de programación principal    |
| Spring Boot       | Framework para desarrollo web         |
| Spring Web        | Para construcción de la API REST      |
| Spring Data JPA   | Acceso a datos con Hibernate          |
| MapStruct         | Mapeo entre entidades y modelos       |
| Swagger / OpenAPI | Documentación de la API               |
| JUnit / Mockito   | Testing.                              |
| Maven             | Gestor de dependencias y construcción |
| PostgreSQL        | Base de datos relacional              |

---

## Estructura del Proyecto

La arquitectura sigue el patrón hexagonal (Ports & Adapters), organizando las responsabilidades en capas claras:

| Carpeta / Archivo                     | Descripción                                                                                        |
| ------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `application/in`                      | Interfaces de entrada (casos de uso). Define los contratos de uso del dominio.                     |
| `application/out`                     | Interfaces de salida (puertos de repositorio). Define cómo el dominio espera acceder a los datos.  |
| `application/usecase`                 | Implementaciones de los casos de uso. Contienen la lógica del negocio.                             |
| `domain/model`                        | Entidades del dominio (p. ej. `User`). Son independientes de frameworks y detalles técnicos.       |
| `infraestructure/delivery/controller` | Controladores REST que reciben las peticiones HTTP y llaman a los casos de uso.                    |
| `infraestructure/entity`              | Entidades JPA para persistencia en base de datos.                                                  |
| `infraestructure/repository/mapper`   | MapStruct Mapper para convertir entre entidades JPA (`UserEntity`) y modelos del dominio (`User`). |
| `infraestructure/repository`          | Implementaciones concretas del repositorio, usando Spring Data JPA.                                |

---

## Endpoints Documentados

| Método | Endpoint          | Descripción                     |
| ------ | ----------------- | ------------------------------- |
| POST   | `/api/users`      | Crear un nuevo usuario          |
| GET    | `/api/users`      | Obtener todos los usuarios      |
| GET    | `/api/users/{id}` | Obtener un usuario por ID       |
| PUT    | `/api/users/{id}` | Actualizar un usuario existente |
| DELETE | `/api/users/{id}` | Eliminar un usuario             |

---

## Documentación OpenAPI

La documentación está generada automáticamente con Swagger usando anotaciones de `springdoc-openapi`. Cada endpoint cuenta con:

* Resumen y descripción.
* Especificación del cuerpo de la petición.
* Ejemplo de JSON.
* Códigos de respuesta esperados (201, 400, 404, 500, etc.).

### Ejemplo de anotación en el Controller:

```java
@Operation(
    summary = "Crear un nuevo usuario",
    requestBody = @io.swagger.v3.oas.annotations.parameters.RequestBody(
        description = "Datos del nuevo usuario",
        required = true,
        content = @Content(
            schema = @Schema(implementation = User.class),
            examples = @ExampleObject(
                name = "Ejemplo de usuario",
                summary = "Un usuario de ejemplo",
                description = "Campos obligatorios para crear un usuario",
                value = "{ \"name\": \"Emilia Perez\", \"email\": \"emilia.perez@example.com\", \"password\": \"123456\" }"
            )
        )
    )
)
```

---

## Principios SOLID Aplicados

| Principio                             | Descripción                                                                | Ejemplo del Proyecto                                                                                                                    |
| ------------------------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| SRP - Single Responsibility Principle | Cada clase debe tener una única responsabilidad.                           | `UserController` solo gestiona solicitudes HTTP. `CreateUserUseCaseImpl` se encarga únicamente del caso de uso "crear usuario".         |
| OCP - Open/Closed Principle           | Las clases deben estar abiertas a extensión, pero cerradas a modificación. | Si se quisiera validar un usuario con una lógica distinta, se puede crear otra clase que implemente `CreateUserUseCase`, sin modificar la actual.                                          |
| LSP - Liskov Substitution Principle   | Las clases derivadas deben poder sustituir a sus clases base.              | Supongamos que `UserRepositoryImpl.findByEmail(email)` retorna `Optional.empty()` si no existe el usuario. Cualquier clase que use `UserRepository` espera esta conducta estándar. Si lanzara una excepción en vez de devolver `Optional.empty()`, se violaría este principio. |
| ISP - Interface Segregation Principle | No forzar a las clases a depender de métodos que no usan.                  | `UserRepository` tiene métodos específicos como `findByFilter`, `save`, `deleteById`, sin agrupar todo en una sola clase enorme.        |
| DIP - Dependency Inversion Principle  | Depender de abstracciones, no de implementaciones concretas.               | `CreateUserUseCaseImpl` depende de `UserService`, que a su vez depende de la interfaz `UserRepository`, no de una clase concreta.       |

---

## Capturas de Swagger UI



---

## Autor

Este proyecto fue desarrollado como ejercicio de arquitectura, aplicación de un stack moderno y buena documentación. Si te interesa, podés escribirme y pedirme el repo con el código de la API RESTful para clonar.

**Autor:** [@marilinagalliano](https://github.com/marilinagalliano)
