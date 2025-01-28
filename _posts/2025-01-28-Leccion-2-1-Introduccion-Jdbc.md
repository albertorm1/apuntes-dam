---
title: Fundamentos de Bases de Datos y JDBC
author: alberto
date: 2025-01-28 00:00:12
categories: ["Semana 2. Bases de Datos Relacionales y JDBC"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Fundamentos de Bases de Datos y JDBC

## 1. Introducción

Las bases de datos son fundamentales para almacenar y gestionar datos en las aplicaciones modernas. En Java, **JDBC (Java Database Connectivity)** permite interactuar con bases de datos relacionales.

En esta lección aprenderemos:
- Qué es JDBC y cómo funciona.
- Conexión a bases de datos en Java.
- Ejecución de consultas SQL básicas desde Java.

---

## 2. ¿Qué es JDBC?

**JDBC (Java Database Connectivity)** es una API de Java que permite conectar aplicaciones con bases de datos relacionales como MySQL, PostgreSQL y SQLite.

### **Componentes de JDBC:**
1. **Driver JDBC:** Biblioteca que facilita la comunicación con la base de datos.
2. **Connection:** Representa una conexión con la base de datos.
3. **Statement:** Ejecuta sentencias SQL.
4. **ResultSet:** Almacena los resultados de una consulta SQL.

---

## 3. Configuración del Entorno JDBC

Para conectarnos a una base de datos necesitamos:
- Un **Driver JDBC** (ejemplo: `mysql-connector-java` para MySQL).
- Agregar la dependencia en el proyecto (Maven o manualmente).

### **Ejemplo de Dependencia para MySQL (Maven)**
Si usamos Maven, agregamos en `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
    </dependency>
</dependencies>
```

Para SQLite, se puede usar:

```xml
<dependency>
    <groupId>org.xerial</groupId>
    <artifactId>sqlite-jdbc</artifactId>
    <version>3.36.0.3</version>
</dependency>
```

Si no usamos Maven, debemos descargar el driver y agregarlo manualmente al classpath.

---

## 4. Conexión a una Base de Datos en Java

### **Ejemplo de Conexión a MySQL**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConexionDB {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa"; // Cambiar según la BD
        String usuario = "root";
        String contraseña = "password";

        try {
            Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
            System.out.println("Conexión exitosa a la base de datos.");
            conexion.close();
        } catch (SQLException e) {
            System.out.println("Error de conexión: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `DriverManager.getConnection(url, usuario, contraseña)`: Establece la conexión con la base de datos.
- `conexion.close()`: Cierra la conexión para liberar recursos.

Para SQLite, la URL sería:
```java
String url = "jdbc:sqlite:empresa.db";
```

---

## 5. Ejecutando Consultas SQL en Java

### **Ejemplo: Creación de Tabla**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class CrearTabla {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";
        
        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement stmt = conexion.createStatement()) {
            
            String sql = "CREATE TABLE IF NOT EXISTS empleados (
                          id INT PRIMARY KEY AUTO_INCREMENT,
                          nombre VARCHAR(100),
                          cargo VARCHAR(100),
                          salario DOUBLE)";
            
            stmt.executeUpdate(sql);
            System.out.println("Tabla creada correctamente.");
        } catch (SQLException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### **Ejemplo: Insertar Datos**

```java
String sql = "INSERT INTO empleados (nombre, cargo, salario) VALUES ('Ana', 'Desarrolladora', 3500)";
stmt.executeUpdate(sql);
```

### **Ejemplo: Consultar Datos**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class LeerDatos {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement stmt = conexion.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM empleados")) {
            
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("id"));
                System.out.println("Nombre: " + rs.getString("nombre"));
                System.out.println("Cargo: " + rs.getString("cargo"));
                System.out.println("Salario: " + rs.getDouble("salario"));
                System.out.println("--------------------");
            }
        } catch (SQLException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
Crear un programa en Java que haga lo siguiente:
1. **Conecte a una base de datos MySQL o SQLite**.
2. **Cree una tabla `productos`** con las columnas: `id`, `nombre`, `precio`.
3. **Inserte al menos 3 productos en la tabla**.
4. **Recupere y muestre los datos en consola**.

**Ejemplo de salida esperada:**
```
ID: 1, Nombre: Laptop, Precio: 1200.00
ID: 2, Nombre: Monitor, Precio: 300.00
ID: 3, Nombre: Teclado, Precio: 50.00
```

**Pistas:** Usa `Statement` para ejecutar las consultas SQL. Asegúrate de manejar excepciones correctamente.

---

Con esto, hemos aprendido a conectar Java con bases de datos usando JDBC y ejecutar consultas SQL básicas. En la próxima clase, veremos cómo mejorar la seguridad y eficiencia con `PreparedStatement`. ¡A programar!
