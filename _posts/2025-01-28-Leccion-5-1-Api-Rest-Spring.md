---
title: Implementación de una API REST Completa con Spring Boot
author: alberto
date: 2025-01-28 00:00:03
categories: ["Semana 5. API REST"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Implementación de una API REST Completa con Spring Boot

## 1. Introducción

En la sesión anterior, aprendimos cómo utilizar **Spring Data JPA** para interactuar con bases de datos. Hoy aprenderemos a construir una **API REST completa** con **Spring Boot**.

En esta lección aprenderemos:
- Qué es una API REST y su estructura.
- Cómo implementar un controlador REST en Spring Boot.
- Cómo manejar respuestas HTTP y excepciones.
- Pruebas de la API con Postman o cURL.

---

## 2. ¿Qué es una API REST?

Una **API REST (Representational State Transfer)** permite la comunicación entre sistemas usando el protocolo HTTP. Utiliza los siguientes métodos HTTP:

| Método     | Propósito             |
| ---------- | --------------------- |
| **GET**    | Obtener datos         |
| **POST**   | Crear nuevos recursos |
| **PUT**    | Actualizar recursos   |
| **DELETE** | Eliminar recursos     |

Ejemplo de endpoints:
- `GET /productos` → Obtener todos los productos.
- `POST /productos` → Crear un nuevo producto.
- `PUT /productos/{id}` → Actualizar un producto existente.
- `DELETE /productos/{id}` → Eliminar un producto.

---

## 3. Configuración del Proyecto

Si aún no tienes Spring Boot configurado, agrega las siguientes dependencias en `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

Configura la base de datos en `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/empresa
spring.datasource.username=root
spring.datasource.password=admin
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## 4. Creación de la Entidad `Producto`

```java
import jakarta.persistence.*;

@Entity
@Table(name = "productos")
public class Producto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nombre;
    private double precio;
    private int stock;

    // Getters y Setters
}
```

---

## 5. Creación del Repositorio `ProductoRepository`

Spring Data JPA nos permite crear un repositorio sin necesidad de escribir código SQL.

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductoRepository extends JpaRepository<Producto, Long> {
}
```

---

## 6. Implementación del Servicio `ProductoService`

El servicio maneja la lógica de negocio.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class ProductoService {
    @Autowired
    private ProductoRepository repository;

    public List<Producto> obtenerTodos() {
        return repository.findAll();
    }

    public Producto guardar(Producto producto) {
        return repository.save(producto);
    }

    public void eliminar(Long id) {
        repository.deleteById(id);
    }
}
```

---

## 7. Creación del Controlador `ProductoController`

El controlador expone la API REST para interactuar con la aplicación.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/productos")
public class ProductoController {
    @Autowired
    private ProductoService service;

    @GetMapping
    public List<Producto> obtenerTodos() {
        return service.obtenerTodos();
    }

    @PostMapping
    public Producto crearProducto(@RequestBody Producto producto) {
        return service.guardar(producto);
    }

    @DeleteMapping("/{id}")
    public void eliminarProducto(@PathVariable Long id) {
        service.eliminar(id);
    }
}
```

---

## 8. Pruebas de la API REST

Podemos probar la API utilizando **Postman** o **cURL**.

### **Ejemplo de prueba con cURL:**

- **Crear un producto:**
```sh
curl -X POST http://localhost:8080/productos -H "Content-Type: application/json" -d '{"nombre":"Laptop","precio":1200.00,"stock":10}'
```

- **Obtener todos los productos:**
```sh
curl -X GET http://localhost:8080/productos
```

- **Eliminar un producto:**
```sh
curl -X DELETE http://localhost:8080/productos/1
```

---

## 9. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear una entidad `Cliente`** con los campos `id`, `nombre`, `email`, `telefono`.
2. **Implementar un repositorio `ClienteRepository`** usando `JpaRepository`.
3. **Crear un servicio `ClienteService`** para manejar la lógica de negocio.
4. **Implementar un controlador `ClienteController`** con endpoints CRUD.

**Ejemplo de salida esperada:**
```
GET /clientes
[
    {"id":1, "nombre":"Juan Pérez", "email":"juan@example.com", "telefono":"555-1234"},
    {"id":2, "nombre":"Ana Gómez", "email":"ana@example.com", "telefono":"555-5678"}
]

POST /clientes
{"id":3, "nombre":"Luis Rodríguez", "email":"luis@example.com", "telefono":"555-9876"}
```

**Pistas:**
- Usa `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` para definir los endpoints.
- Usa `@Autowired` para inyectar dependencias.

---

Con esta lección, hemos aprendido a construir una API REST con **Spring Boot**, exponiendo operaciones CRUD para interactuar con una base de datos. En la próxima sesión, agregaremos **seguridad y autenticación con JWT**. ¡A programar!
