---
title: Introducción a Spring Data JPA
author: alberto
date: 2025-01-28 00:00:04
categories: ["Semana 4. Bases de Datos NoSQL y Spring Data JPA"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Introducción a Spring Data JPA

## 1. Introducción

Spring Data JPA es un módulo de Spring Boot que simplifica el acceso a bases de datos relacionales mediante JPA (Java Persistence API). En esta lección aprenderemos:
- Qué es Spring Data JPA y cómo funciona.
- Cómo configurar Spring Boot con JPA.
- Cómo realizar operaciones CRUD con `JpaRepository`.
- Uso de consultas personalizadas con Spring Data.

---

## 2. ¿Qué es Spring Data JPA?

Spring Data JPA permite interactuar con bases de datos relacionales de manera declarativa, sin necesidad de escribir código JDBC o Hibernate manualmente.

### **Beneficios de Spring Data JPA:**
- **Menos código**: No es necesario escribir consultas SQL manuales.
- **Integración con Spring Boot**: Configuración automática.
- **Consultas personalizadas**: Métodos de consulta derivados de nombres de métodos.

---

## 3. Configuración del Proyecto

Si usamos Maven, debemos agregar las siguientes dependencias en `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

Además, configuramos la conexión a la base de datos en `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/empresa
spring.datasource.username=root
spring.datasource.password=admin
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

## 4. Creación de una Entidad con JPA

Definimos una entidad `Empleado` que representa la tabla `empleados` en la base de datos:

```java
import jakarta.persistence.*;

@Entity
@Table(name = "empleados")
public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String nombre;
    private String cargo;
    private double salario;

    // Getters y Setters
}
```

### **Explicación:**
- `@Entity`: Indica que la clase es una entidad JPA.
- `@Table(name = "empleados")`: Especifica el nombre de la tabla.
- `@Id` y `@GeneratedValue`: Indican la clave primaria autogenerada.

---

## 5. Creación del Repositorio con `JpaRepository`

Spring Data JPA proporciona `JpaRepository` para realizar operaciones CRUD sin necesidad de escribir código SQL.

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface EmpleadoRepository extends JpaRepository<Empleado, Long> {
}
```

---

## 6. Creación del Servicio y Controlador

Para manejar la lógica de negocio, creamos un servicio:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class EmpleadoService {
    @Autowired
    private EmpleadoRepository repository;

    public List<Empleado> obtenerTodos() {
        return repository.findAll();
    }

    public Empleado guardar(Empleado empleado) {
        return repository.save(empleado);
    }
}
```

Luego, creamos un controlador REST para exponer las operaciones CRUD:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/empleados")
public class EmpleadoController {
    @Autowired
    private EmpleadoService service;

    @GetMapping
    public List<Empleado> obtenerTodos() {
        return service.obtenerTodos();
    }

    @PostMapping
    public Empleado crearEmpleado(@RequestBody Empleado empleado) {
        return service.guardar(empleado);
    }
}
```

---

## 7. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear una entidad `Producto`** con los campos `id`, `nombre`, `precio`, `stock`.
2. **Implementar un repositorio `ProductoRepository`** usando `JpaRepository`.
3. **Crear un servicio `ProductoService`** para manejar la lógica de negocio.
4. **Implementar un controlador `ProductoController`** con endpoints CRUD.

**Ejemplo de salida esperada:**
```
GET /productos
[
    {"id":1, "nombre":"Laptop", "precio":1200.00, "stock":10},
    {"id":2, "nombre":"Monitor", "precio":300.00, "stock":5}
]

POST /productos
{"id":3, "nombre":"Teclado", "precio":50.00, "stock":20}
```

**Pistas:**
- Usa `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` para definir los endpoints.
- Usa `@Autowired` para inyectar dependencias.

---

Con esta lección, hemos aprendido cómo integrar Spring Data JPA en una aplicación Spring Boot para gestionar bases de datos de forma sencilla. En la próxima sesión, veremos cómo agregar autenticación y seguridad con JWT. ¡A programar!
