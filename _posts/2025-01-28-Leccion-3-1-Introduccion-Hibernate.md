---
title: Introducción a Hibernate
author: alberto
date: 2025-01-28 00:00:09
categories: ["Semana 3. ORM con Hibernate"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Introducción a Hibernate

## 1. Introducción

**Hibernate** es un framework de **ORM (Object-Relational Mapping)** que permite a las aplicaciones en Java interactuar con bases de datos de manera eficiente, sin necesidad de escribir sentencias SQL manualmente.

En esta lección aprenderemos:
- Qué es Hibernate y por qué usarlo.
- Instalación y configuración de Hibernate.
- Creación de entidades y mapeo a tablas de bases de datos.

---

## 2. ¿Qué es Hibernate?

Hibernate facilita la gestión de bases de datos en Java mediante el uso de **POJOs (Plain Old Java Objects)** que representan las tablas de la base de datos. 

### **Ventajas de Hibernate:**
- **Independencia de la base de datos**: Funciona con MySQL, PostgreSQL, SQLite, etc.
- **Evita SQL manual**: Utiliza **HQL (Hibernate Query Language)** y consultas con objetos.
- **Gestión automática de transacciones y conexiones**.
- **Manejo de relaciones entre entidades** (1:N, N:M).

---

## 3. Instalación y Configuración de Hibernate

Para usar Hibernate en un proyecto Java con Maven, agregamos las dependencias necesarias en el archivo `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>6.2.0.Final</version>
    </dependency>
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.6.15.Final</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.30</version>
    </dependency>
</dependencies>
```

También necesitamos un archivo de configuración llamado `hibernate.cfg.xml` en `src/main/resources`:

```xml
<!DOCTYPE hibernate-configuration>
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/empresa</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">password</property>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="hibernate.hbm2ddl.auto">update</property>
        <property name="hibernate.show_sql">true</property>
    </session-factory>
</hibernate-configuration>
```

---

## 4. Creación de Entidades en Hibernate

Para mapear una tabla de base de datos a una clase en Java, utilizamos anotaciones de Hibernate.

### **Ejemplo: Creación de la Entidad `Empleado`**

```java
import jakarta.persistence.*;

@Entity
@Table(name = "empleados")
public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    
    @Column(name = "nombre")
    private String nombre;
    
    @Column(name = "cargo")
    private String cargo;
    
    @Column(name = "salario")
    private double salario;

    // Constructores, Getters y Setters
    public Empleado() {}
    
    public Empleado(String nombre, String cargo, double salario) {
        this.nombre = nombre;
        this.cargo = cargo;
        this.salario = salario;
    }

    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }

    public String getCargo() { return cargo; }
    public void setCargo(String cargo) { this.cargo = cargo; }

    public double getSalario() { return salario; }
    public void setSalario(double salario) { this.salario = salario; }
}
```

### **Explicación:**
- `@Entity`: Indica que esta clase es una entidad de Hibernate.
- `@Table(name = "empleados")`: Especifica el nombre de la tabla en la base de datos.
- `@Id`: Indica la clave primaria.
- `@GeneratedValue(strategy = GenerationType.IDENTITY)`: Genera valores automáticamente.
- `@Column(name = "campo")`: Mapea los atributos a las columnas de la tabla.

---

## 5. Creación de una Sesión y Transacciones en Hibernate

Para interactuar con la base de datos, usamos una **sesión de Hibernate**.

### **Ejemplo: Guardar un Empleado en la Base de Datos**

```java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

public class HibernateMain {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Empleado.class).buildSessionFactory();
        Session session = factory.getCurrentSession();
        
        try {
            // Crear un nuevo empleado
            Empleado nuevoEmpleado = new Empleado("Ana", "Desarrolladora", 4500);
            
            // Iniciar transacción
            session.beginTransaction();
            
            // Guardar el empleado
            session.save(nuevoEmpleado);
            
            // Confirmar transacción
            session.getTransaction().commit();
            
            System.out.println("Empleado guardado correctamente.");
        } finally {
            factory.close();
        }
    }
}
```

### **Explicación:**
- `SessionFactory`: Crea sesiones para interactuar con la base de datos.
- `session.beginTransaction()`: Inicia una transacción.
- `session.save(objeto)`: Guarda un objeto en la base de datos.
- `session.getTransaction().commit()`: Confirma los cambios.

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
Crear una aplicación en Java que haga lo siguiente:
1. **Cree una tabla `productos`** con las columnas `id`, `nombre`, `precio`.
2. **Inserte al menos 3 productos en la base de datos usando Hibernate**.
3. **Recupere y muestre los productos en la consola**.

**Ejemplo de salida esperada:**
```
Producto guardado correctamente.
ID: 1, Nombre: Laptop, Precio: 1200.00
ID: 2, Nombre: Monitor, Precio: 300.00
ID: 3, Nombre: Teclado, Precio: 50.00
```

**Pistas:** Usa `@Entity`, `@Table`, `@Column` y una sesión de Hibernate para interactuar con la base de datos.

---

Con esta lección, hemos aprendido los fundamentos de Hibernate y cómo mapear clases Java a tablas de base de datos. En la próxima sesión, veremos cómo hacer consultas avanzadas con Hibernate. ¡A programar!
