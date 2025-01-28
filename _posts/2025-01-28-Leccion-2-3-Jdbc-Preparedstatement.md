---
title: PreparedStatement y Manejo de Excepciones en JDBC
author: alberto
date: 2025-01-28 00:00:10
categories: ["Semana 2. Bases de Datos Relacionales y JDBC"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Consultas SQL y CRUD en Java con JDBC

# PreparedStatement y Manejo de Excepciones en JDBC

## 1. Introducción

En la sesión anterior, aprendimos a ejecutar consultas SQL en Java utilizando `Statement`. Sin embargo, esta aproximación tiene limitaciones, como la vulnerabilidad a inyecciones SQL y la dificultad de reutilizar consultas con diferentes parámetros.

Para solucionar esto, en esta lección aprenderemos:
- Qué es `PreparedStatement` y por qué es más seguro.
- Cómo utilizar `PreparedStatement` para consultas parametrizadas.
- Manejo adecuado de excepciones en JDBC.

---

## 2. ¿Qué es `PreparedStatement` y por qué usarlo?

`PreparedStatement` es una subclase de `Statement` que permite ejecutar consultas SQL parametrizadas, evitando la concatenación de cadenas y reduciendo el riesgo de inyección SQL.

### **Beneficios de `PreparedStatement`:**
- **Seguridad**: Evita inyecciones SQL.
- **Rendimiento**: Las consultas pueden ser compiladas y reutilizadas.
- **Legibilidad**: Código más limpio y estructurado.

### **Ejemplo de Vulnerabilidad con `Statement` (Mala Práctica)**
```java
String usuario = "admin";
String password = "' OR '1'='1"; // Inyección SQL

String sql = "SELECT * FROM usuarios WHERE username = '" + usuario + "' AND password = '" + password + "'";
Statement stmt = conexion.createStatement();
ResultSet rs = stmt.executeQuery(sql);
```

Esta consulta permitiría acceder sin credenciales válidas. En su lugar, debemos usar `PreparedStatement`.

---

## 3. Uso de `PreparedStatement` en Java

### **Ejemplo: Inserción Segura con `PreparedStatement`**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class InsertarSeguro {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";
        
        String sql = "INSERT INTO empleados (nombre, cargo, salario) VALUES (?, ?, ?)";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             PreparedStatement pstmt = conexion.prepareStatement(sql)) {
            
            pstmt.setString(1, "Pedro");
            pstmt.setString(2, "Gerente");
            pstmt.setDouble(3, 5000);
            
            int filasAfectadas = pstmt.executeUpdate();
            System.out.println("Filas insertadas: " + filasAfectadas);
        } catch (SQLException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `?` son marcadores de posición para parámetros dinámicos.
- `setString()`, `setDouble()`, etc., asignan valores seguros a los parámetros.
- `executeUpdate()` ejecuta la consulta y devuelve el número de filas afectadas.

---

## 4. Consultas con `PreparedStatement`

Podemos usar `PreparedStatement` para consultas `SELECT` también.

### **Ejemplo: Consulta con Parámetros**

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ConsultarEmpleados {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/empresa";
        String usuario = "root";
        String contraseña = "password";
        
        String sql = "SELECT * FROM empleados WHERE cargo = ?";

        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             PreparedStatement pstmt = conexion.prepareStatement(sql)) {
            
            pstmt.setString(1, "Gerente");
            
            ResultSet rs = pstmt.executeQuery();
            
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("id"));
                System.out.println("Nombre: " + rs.getString("nombre"));
                System.out.println("Salario: " + rs.getDouble("salario"));
                System.out.println("----------------");
            }
        } catch (SQLException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### **Explicación:**
- `?` en la consulta evita concatenaciones inseguras.
- `setString(1, "Gerente")` asigna el valor al primer parámetro.
- `executeQuery()` devuelve un `ResultSet` con los resultados.

---

## 5. Manejo de Excepciones en JDBC

Es importante manejar errores adecuadamente para evitar fallos inesperados.

### **Principales excepciones en JDBC:**
- `SQLException`: Error en la conexión o en la ejecución de SQL.
- `SQLIntegrityConstraintViolationException`: Violación de restricciones (ejemplo: clave primaria duplicada).
- `SQLSyntaxErrorException`: Error en la sintaxis de SQL.

### **Ejemplo de Manejo de Excepciones en JDBC**

```java
import java.sql.*;

public class ManejoExcepciones {
    public static void main(String[] args) {
        try {
            Connection conexion = DriverManager.getConnection("jdbc:mysql://localhost:3306/empresa", "root", "password");
            Statement stmt = conexion.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM empleados");
            
            while (rs.next()) {
                System.out.println("Empleado: " + rs.getString("nombre"));
            }
        } catch (SQLIntegrityConstraintViolationException e) {
            System.out.println("Error: Clave duplicada - " + e.getMessage());
        } catch (SQLSyntaxErrorException e) {
            System.out.println("Error: Sintaxis SQL incorrecta - " + e.getMessage());
        } catch (SQLException e) {
            System.out.println("Error general de base de datos: " + e.getMessage());
        }
    }
}
```

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
Crear un programa en Java que haga lo siguiente:
1. **Cree una tabla `clientes`** con los campos `id`, `nombre`, `email`, `telefono`.
2. **Inserte al menos 3 clientes** usando `PreparedStatement`.
3. **Recupere y muestre los datos** de la tabla.
4. **Actualice un número de teléfono** basándose en el `id`.
5. **Elimine un cliente** usando su `id`.
6. **Maneje correctamente las excepciones JDBC**.

**Ejemplo de salida esperada:**
```
Cliente insertado correctamente.
ID: 1, Nombre: Juan, Email: juan@example.com, Teléfono: 555-1234
ID: 2, Nombre: Ana, Email: ana@example.com, Teléfono: 555-5678
Cliente actualizado correctamente.
Cliente eliminado correctamente.
```

**Pistas:** Usa `PreparedStatement` para todas las consultas y maneja excepciones adecuadamente.

---

Con esto, hemos aprendido a mejorar la seguridad y eficiencia en el acceso a bases de datos con `PreparedStatement` y manejo de excepciones. En la próxima clase, exploraremos Hibernate como alternativa a JDBC. ¡A programar!
