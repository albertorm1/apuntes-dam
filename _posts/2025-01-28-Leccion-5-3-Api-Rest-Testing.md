---
title: Pruebas Automáticas para una API REST con Spring Boot
author: alberto
date: 2025-01-28 00:00:01
categories: ["Semana 5. API REST"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Pruebas Automáticas para una API REST con Spring Boot

## 1. Introducción

Para garantizar la calidad y funcionalidad de nuestra API REST, es fundamental realizar **pruebas automatizadas**. Hoy aprenderemos:
- Cómo realizar pruebas unitarias con **JUnit y Mockito**.
- Cómo probar controladores REST con **Spring Boot Test y MockMvc**.
- Cómo ejecutar pruebas de integración en la API REST.

---

## 2. Configuración del Proyecto para Pruebas

Asegúrate de agregar las siguientes dependencias en `pom.xml`:

```xml
<dependencies>
    <!-- Spring Boot Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <!-- Mockito -->
    <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-core</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

## 3. Pruebas Unitarias con JUnit y Mockito

Las pruebas unitarias permiten verificar el comportamiento de **clases individuales** sin necesidad de ejecutar toda la aplicación.

### **Ejemplo: Prueba Unitaria para `ProductoService`**

```java
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import java.util.Arrays;
import java.util.List;

public class ProductoServiceTest {
    @Mock
    private ProductoRepository productoRepository;

    @InjectMocks
    private ProductoService productoService;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testObtenerTodos() {
        when(productoRepository.findAll()).thenReturn(
            Arrays.asList(new Producto(1L, "Laptop", 1200.00, 10)));

        List<Producto> productos = productoService.obtenerTodos();

        assertFalse(productos.isEmpty());
        assertEquals(1, productos.size());
        assertEquals("Laptop", productos.get(0).getNombre());
    }
}
```

### **Explicación:**
- `@Mock` crea un objeto simulado de `ProductoRepository`.
- `@InjectMocks` inyecta el mock en `ProductoService`.
- `when(...).thenReturn(...)` define el comportamiento del mock.
- `assertEquals(...)` verifica los resultados esperados.

---

## 4. Pruebas de Controladores con MockMvc

`MockMvc` permite simular peticiones HTTP y probar los endpoints de nuestros controladores.

### **Ejemplo: Prueba para `ProductoController`**

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.*;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(ProductoController.class)
public class ProductoControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    void testObtenerTodos() throws Exception {
        mockMvc.perform(get("/productos"))
            .andExpect(status().isOk())
            .andExpect(content().contentType("application/json"));
    }
}
```

### **Explicación:**
- `@WebMvcTest(ProductoController.class)`: Carga solo el controlador para pruebas.
- `mockMvc.perform(get("/productos"))`: Simula una petición `GET`.
- `.andExpect(status().isOk())`: Verifica que la respuesta HTTP es 200.
- `.andExpect(content().contentType("application/json"))`: Verifica que la respuesta sea JSON.

---

## 5. Pruebas de Integración en Spring Boot

Las pruebas de integración verifican cómo interactúan los **diferentes componentes** de la aplicación.

### **Ejemplo: Prueba de Integración para `ProductoRepository`**

```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.orm.jpa.DataJpaTest;
import org.springframework.boot.test.context.SpringBootTest;
import java.util.List;

@DataJpaTest
public class ProductoRepositoryTest {
    @Autowired
    private ProductoRepository productoRepository;

    @Test
    void testGuardarYRecuperarProducto() {
        Producto producto = new Producto(null, "Teclado", 50.00, 20);
        productoRepository.save(producto);

        List<Producto> productos = productoRepository.findAll();
        assertFalse(productos.isEmpty());
    }
}
```

### **Explicación:**
- `@DataJpaTest` configura un entorno de prueba para JPA.
- `productoRepository.save(producto)`: Guarda un producto en la base de datos.
- `productoRepository.findAll()`: Recupera los productos guardados.

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear pruebas unitarias** para `ClienteService` usando `Mockito`.
2. **Escribir pruebas para `ClienteController`** con `MockMvc`.
3. **Implementar una prueba de integración** para `ClienteRepository`.

**Ejemplo de salida esperada:**
```
✔ Prueba unitaria ClienteService - OK
✔ Prueba de controlador ClienteController - OK
✔ Prueba de integración ClienteRepository - OK
```

**Pistas:**
- Usa `@Mock` y `@InjectMocks` para los servicios.
- Usa `mockMvc.perform(get("/clientes"))` para probar el controlador.
- Usa `@DataJpaTest` para probar el repositorio.

---

## 7. Resumen

Hoy hemos aprendido:
✔ Cómo escribir pruebas unitarias con **JUnit y Mockito**.
✔ Cómo probar controladores REST con **MockMvc**.
✔ Cómo realizar pruebas de integración con **Spring Boot**.

Las pruebas automáticas son esenciales para mantener la calidad del código y evitar errores en producción. ¡En la próxima sesión, realizaremos un **proyecto final integrando todo lo aprendido**! 🚀
