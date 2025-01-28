---
title: Consultas SQL y CRUD en Java con JDBC
author: alberto
date: 2025-01-28 00:00:11
categories: ["Semana 2. Bases de Datos Relacionales y JDBC"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Consultas SQL y CRUD en Java con JDBC

## 1. Introducción

En la clase anterior aprendimos cómo conectar Java a una base de datos utilizando JDBC. Hoy exploraremos cómo realizar operaciones CRUD (**Create, Read, Update, Delete**) en una base de datos desde Java.

En esta lección aprenderemos:
- Cómo insertar datos en una base de datos.
- Cómo recuperar datos y mostrarlos en la consola.
- Cómo actualizar y eliminar registros.
- Buenas prácticas en el uso de JDBC.

---

## 2. Insertar Datos en la Base de Datos

Para agregar datos a una tabla utilizamos la sentencia `INSERT INTO` dentro de Java con JDBC.

### **Ejemplo: Insertar un Registro en la Tabla `empleados`**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class InsertarDatos {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement stmt = conexion.createStatement()) {
            
            String sql = "INSERT INTO empleados (nombre, cargo, salario) VALUES ('Carlos', 'Ingeniero', 4000)";
            stmt.executeUpdate(sql);
            System.out.println("Registro insertado correctamente.");
        } catch (SQLException e) {
            System.out.println("Error al insertar datos: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `stmt.executeUpdate(sql)`: Ejecuta una consulta de modificación de datos (`INSERT`, `UPDATE`, `DELETE`).
- La consulta inserta un nuevo empleado en la base de datos.

---

## 3. Leer Datos desde la Base de Datos

Para recuperar datos usamos la sentencia `SELECT` y un objeto `ResultSet`.

### **Ejemplo: Obtener Todos los Empleados**

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
            System.out.println("Error al leer los datos: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `stmt.executeQuery(sql)`: Ejecuta una consulta de `SELECT` y devuelve un `ResultSet`.
- `rs.next()`: Avanza por los registros obtenidos.
- `rs.getString("nombre")`: Obtiene valores por nombre de columna.

---

## 4. Actualizar Datos en la Base de Datos

Para modificar datos usamos la sentencia `UPDATE`.

### **Ejemplo: Actualizar el Salario de un Empleado**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class ActualizarDatos {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement stmt = conexion.createStatement()) {
            
            String sql = "UPDATE empleados SET salario = 4500 WHERE nombre = 'Carlos'";
            stmt.executeUpdate(sql);
            System.out.println("Registro actualizado correctamente.");
        } catch (SQLException e) {
            System.out.println("Error al actualizar datos: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `UPDATE empleados SET salario = 4500 WHERE nombre = 'Carlos'` modifica el salario de un empleado específico.

---

## 5. Eliminar Datos de la Base de Datos

Para borrar registros usamos la sentencia `DELETE`.

### **Ejemplo: Eliminar un Empleado**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class EliminarDatos {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement stmt = conexion.createStatement()) {
            
            String sql = "DELETE FROM empleados WHERE nombre = 'Carlos'";
            stmt.executeUpdate(sql);
            System.out.println("Registro eliminado correctamente.");
        } catch (SQLException e) {
            System.out.println("Error al eliminar datos: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `DELETE FROM empleados WHERE nombre = 'Carlos'` elimina un empleado específico de la tabla.

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
Crear un programa en Java que haga lo siguiente:
1. **Cree una tabla `productos`** con las columnas `id`, `nombre`, `precio`.
2. **Inserte al menos 3 productos en la tabla**.
3. **Recupere y muestre los productos en la consola**.
4. **Actualice el precio de un producto** basado en su `id`.
5. **Elimine un producto de la tabla**.

**Ejemplo de salida esperada:**
```
Producto insertado correctamente.
ID: 1, Nombre: Laptop, Precio: 1200.00
ID: 2, Nombre: Monitor, Precio: 300.00
ID: 3, Nombre: Teclado, Precio: 50.00
Producto actualizado correctamente.
Producto eliminado correctamente.
```

**Pistas:** Usa `Statement` para ejecutar consultas SQL. Asegúrate de manejar excepciones correctamente.

---

Con esto, hemos aprendido a realizar operaciones CRUD en bases de datos con JDBC. En la próxima clase, veremos cómo mejorar la seguridad con `PreparedStatement`. ¡A programar!
