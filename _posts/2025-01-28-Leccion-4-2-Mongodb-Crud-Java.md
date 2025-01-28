---
title: Operaciones CRUD en MongoDB con Java
author: alberto
date: 2025-01-28 00:00:05
categories: ["Semana 4. Bases de Datos NoSQL y Spring Data JPA"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Operaciones CRUD en MongoDB con Java

## 1. Introducción

En la sesión anterior aprendimos cómo conectar Java con MongoDB. Hoy nos enfocaremos en cómo realizar operaciones **CRUD** (**Create, Read, Update, Delete**) en MongoDB utilizando Java.

En esta lección aprenderemos:
- Cómo insertar documentos en MongoDB desde Java.
- Cómo consultar datos en MongoDB.
- Cómo actualizar documentos existentes.
- Cómo eliminar documentos de una colección.

---

## 2. Configuración del Proyecto

Si aún no lo has hecho, asegúrate de agregar la dependencia del driver de MongoDB en `pom.xml`:

```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-sync</artifactId>
    <version>4.7.2</version>
</dependency>
```

---

## 3. Insertar Documentos en MongoDB

### **Ejemplo: Insertar un Documento en la Colección `productos`**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoCollection;
import org.bson.Document;

public class InsertarProducto {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("tienda");
            MongoCollection<Document> collection = database.getCollection("productos");

            Document producto = new Document("nombre", "Laptop")
                    .append("precio", 1200.00)
                    .append("stock", 10);
            
            collection.insertOne(producto);
            System.out.println("Producto insertado correctamente.");
        }
    }
}
```

### **Explicación:**
- Se obtiene la base de datos `tienda` y la colección `productos`.
- Se crea un documento JSON con `Document`.
- `collection.insertOne(producto)` inserta el documento en la colección.

---

## 4. Consultar Documentos en MongoDB

### **Ejemplo: Obtener Todos los Productos**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.FindIterable;
import org.bson.Document;

public class ConsultarProductos {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("tienda");
            MongoCollection<Document> collection = database.getCollection("productos");

            FindIterable<Document> productos = collection.find();
            for (Document doc : productos) {
                System.out.println(doc.toJson());
            }
        }
    }
}
```

### **Explicación:**
- `collection.find()` obtiene todos los documentos de la colección.
- Se recorren los resultados e imprimen en formato JSON.

---

## 5. Actualizar Documentos en MongoDB

### **Ejemplo: Actualizar el Precio de un Producto**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoCollection;
import org.bson.Document;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;

public class ActualizarProducto {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("tienda");
            MongoCollection<Document> collection = database.getCollection("productos");

            collection.updateOne(Filters.eq("nombre", "Laptop"), Updates.set("precio", 1100.00));
            System.out.println("Producto actualizado correctamente.");
        }
    }
}
```

### **Explicación:**
- `Filters.eq("nombre", "Laptop")` busca el producto por su nombre.
- `Updates.set("precio", 1100.00)` modifica el precio.
- `updateOne()` actualiza un solo documento.

---

## 6. Eliminar Documentos en MongoDB

### **Ejemplo: Eliminar un Producto**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoCollection;
import org.bson.Document;
import com.mongodb.client.model.Filters;

public class EliminarProducto {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("tienda");
            MongoCollection<Document> collection = database.getCollection("productos");

            collection.deleteOne(Filters.eq("nombre", "Laptop"));
            System.out.println("Producto eliminado correctamente.");
        }
    }
}
```

### **Explicación:**
- `Filters.eq("nombre", "Laptop")` encuentra el producto a eliminar.
- `deleteOne()` elimina el primer documento que coincida con el filtro.

---

## 7. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear una base de datos `biblioteca` y una colección `libros`.**
2. **Insertar al menos 3 libros en la colección** con los campos `titulo`, `autor` y `año`.
3. **Consultar y mostrar todos los libros almacenados.**
4. **Actualizar el año de publicación de un libro específico.**
5. **Eliminar un libro de la colección.**

**Ejemplo de salida esperada:**
```
Libro insertado correctamente.
Título: "El Quijote" - Autor: "Miguel de Cervantes" - Año: 1605
Libro actualizado correctamente.
Libro eliminado correctamente.
```

**Pistas:** Usa `insertOne()`, `find()`, `updateOne()` y `deleteOne()` en MongoDB Java Driver.

---

Con esta lección, hemos aprendido a realizar operaciones CRUD en MongoDB desde Java. En la próxima sesión, exploraremos cómo integrar MongoDB con Spring Boot. ¡A programar!
