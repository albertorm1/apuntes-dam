---
title: Introducción a MongoDB y Acceso con Java
author: alberto
date: 2025-01-28 00:00:06
categories: ["Semana 4. Bases de Datos NoSQL y Spring Data JPA"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Introducción a MongoDB y Acceso con Java

## 1. Introducción

MongoDB es una base de datos **NoSQL** que almacena datos en formato **JSON/BSON**, lo que permite mayor flexibilidad en el diseño de esquemas. En esta lección aprenderemos:
- Qué es MongoDB y cómo se diferencia de las bases de datos relacionales.
- Instalación y configuración del driver de MongoDB en Java.
- Cómo realizar operaciones CRUD en MongoDB usando Java.

---

## 2. ¿Qué es MongoDB?

MongoDB es una base de datos orientada a documentos que **no usa tablas ni SQL**. En lugar de ello, almacena datos en colecciones de documentos JSON/BSON.

### **Comparación entre MongoDB y Bases de Datos Relacionales**
| Característica  | MongoDB                                               | SQL (MySQL, PostgreSQL)    |
| --------------- | ----------------------------------------------------- | -------------------------- |
| Modelo de datos | Documentos (JSON/BSON)                                | Tablas y filas             |
| Esquema         | Flexible (sin necesidad de definir estructuras fijas) | Rígido (estructuras fijas) |
| Consultas       | Basadas en documentos                                 | Basadas en SQL             |
| Escalabilidad   | Horizontal (sharding)                                 | Vertical (normalización)   |

### **Ejemplo de Documento en MongoDB (JSON/BSON)**
```json
{
    "_id": ObjectId("609d5a88f123456789abcd12"),
    "nombre": "Juan",
    "edad": 30,
    "cargo": "Desarrollador",
    "habilidades": ["Java", "Spring Boot", "MongoDB"]
}
```

---

## 3. Configuración del Driver de MongoDB en Java

Para conectarnos a MongoDB en Java, usamos el **MongoDB Java Driver**.

### **Dependencia en Maven**
Agregamos el siguiente código en `pom.xml`:
```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-sync</artifactId>
    <version>4.7.2</version>
</dependency>
```

---

## 4. Conexión a MongoDB desde Java

### **Ejemplo: Conectar a una Base de Datos MongoDB**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoDatabase;

public class ConexionMongoDB {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("empresa");
            System.out.println("Conectado a la base de datos: " + database.getName());
        }
    }
}
```

### **Explicación:**
- `MongoClients.create("mongodb://localhost:27017")` establece la conexión con MongoDB en el puerto `27017`.
- `mongoClient.getDatabase("empresa")` obtiene la base de datos `empresa`.
- La conexión se maneja dentro de `try-with-resources` para su cierre automático.

---

## 5. Operaciones CRUD en MongoDB con Java

### **5.1 Insertar Documentos**
```java
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

public class InsertarDocumento {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("empresa");
            MongoCollection<Document> collection = database.getCollection("empleados");

            Document empleado = new Document("nombre", "Ana")
                    .append("edad", 28)
                    .append("cargo", "Analista")
                    .append("habilidades", List.of("SQL", "MongoDB"));

            collection.insertOne(empleado);
            System.out.println("Documento insertado correctamente.");
        }
    }
}
```

### **5.2 Consultar Documentos**
```java
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoClients;
import org.bson.Document;
import com.mongodb.client.FindIterable;

public class ConsultarDocumentos {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("empresa");
            MongoCollection<Document> collection = database.getCollection("empleados");

            FindIterable<Document> empleados = collection.find();
            for (Document doc : empleados) {
                System.out.println(doc.toJson());
            }
        }
    }
}
```

### **5.3 Actualizar Documentos**
```java
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoClients;
import org.bson.Document;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;

public class ActualizarDocumento {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("empresa");
            MongoCollection<Document> collection = database.getCollection("empleados");

            collection.updateOne(Filters.eq("nombre", "Ana"), Updates.set("cargo", "Gerente"));
            System.out.println("Documento actualizado correctamente.");
        }
    }
}
```

### **5.4 Eliminar Documentos**
```java
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoClients;
import org.bson.Document;
import com.mongodb.client.model.Filters;

public class EliminarDocumento {
    public static void main(String[] args) {
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            MongoDatabase database = mongoClient.getDatabase("empresa");
            MongoCollection<Document> collection = database.getCollection("empleados");

            collection.deleteOne(Filters.eq("nombre", "Ana"));
            System.out.println("Documento eliminado correctamente.");
        }
    }
}
```

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear una base de datos `tienda` y una colección `productos`**.
2. **Insertar al menos 3 productos en la colección** con los campos `nombre`, `precio` y `stock`.
3. **Consultar y mostrar los productos almacenados**.
4. **Actualizar el precio de un producto específico**.
5. **Eliminar un producto de la colección**.

**Ejemplo de salida esperada:**
```
Producto insertado correctamente.
Nombre: Laptop - Precio: 1200.00 - Stock: 10
Producto actualizado correctamente.
Producto eliminado correctamente.
```

**Pistas:** Usa `insertOne()`, `find()`, `updateOne()` y `deleteOne()` en MongoDB Java Driver.

---

Con esta lección, hemos aprendido cómo interactuar con MongoDB desde Java. En la próxima clase, exploraremos cómo realizar consultas avanzadas y agregaciones en MongoDB. ¡A programar!
