---
title: Relaciones en Hibernate
author: alberto
date: 2025-01-28 00:00:08
categories: ["Semana 3. ORM con Hibernate"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Relaciones en Hibernate

## 1. Introducción

En la sesión anterior, aprendimos los conceptos básicos de Hibernate y cómo mapear una clase a una tabla de base de datos. Hoy exploraremos cómo modelar relaciones entre entidades en Hibernate.

En esta lección aprenderemos:
- Relaciones **Uno a Muchos (1:N)** y **Muchos a Uno (N:1)**.
- Relaciones **Muchos a Muchos (N:M)**.
- Uso de `@OneToMany`, `@ManyToOne`, y `@ManyToMany`.
- Cómo persistir y recuperar datos en relaciones.

---

## 2. Relaciones en Bases de Datos

Las bases de datos relacionales utilizan claves foráneas para definir relaciones entre tablas. Hibernate nos permite modelar estas relaciones directamente en nuestras clases usando anotaciones.

### **Tipos de Relaciones**

| Tipo                      | Descripción                                                                                    |
| ------------------------- | ---------------------------------------------------------------------------------------------- |
| **Uno a Muchos (1:N)**    | Un registro en una tabla puede estar asociado con múltiples registros en otra tabla.           |
| **Muchos a Uno (N:1)**    | Múltiples registros en una tabla pueden estar asociados a un solo registro en otra tabla.      |
| **Muchos a Muchos (N:M)** | Múltiples registros de una tabla pueden estar asociados con múltiples registros de otra tabla. |

---

## 3. Relación Uno a Muchos (1:N) y Muchos a Uno (N:1)

Supongamos que tenemos una empresa donde **un departamento** puede tener **muchos empleados**.

### **Entidad `Departamento` (1:N con Empleados)**

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "departamentos")
public class Departamento {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "nombre")
    private String nombre;

    @OneToMany(mappedBy = "departamento", cascade = CascadeType.ALL)
    private List<Empleado> empleados;

    // Getters y Setters
}
```

### **Entidad `Empleado` (N:1 con Departamento)**

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

    @ManyToOne
    @JoinColumn(name = "departamento_id")
    private Departamento departamento;

    // Getters y Setters
}
```

### **Explicación:**
- `@OneToMany(mappedBy = "departamento")`: Define una relación **Uno a Muchos** en `Departamento`.
- `@ManyToOne` en `Empleado` indica que **varios empleados** pueden estar en **un solo departamento**.
- `@JoinColumn(name = "departamento_id")`: Define la clave foránea en `empleados`.

---

## 4. Relación Muchos a Muchos (N:M)

Si queremos modelar una relación donde **un empleado puede trabajar en varios proyectos** y **un proyecto puede tener varios empleados**, usamos `@ManyToMany`.

### **Entidad `Proyecto`**

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "proyectos")
public class Proyecto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "nombre")
    private String nombre;

    @ManyToMany
    @JoinTable(
        name = "empleado_proyecto",
        joinColumns = @JoinColumn(name = "proyecto_id"),
        inverseJoinColumns = @JoinColumn(name = "empleado_id")
    )
    private List<Empleado> empleados;

    // Getters y Setters
}
```

### **Modificar `Empleado` para la Relación N:M**

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "empleados")
public class Empleado {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "nombre")
    private String nombre;

    @ManyToMany(mappedBy = "empleados")
    private List<Proyecto> proyectos;

    // Getters y Setters
}
```

### **Explicación:**
- `@ManyToMany`: Define una relación de **muchos a muchos**.
- `@JoinTable`: Especifica la tabla intermedia `empleado_proyecto`.
- `mappedBy = "empleados"`: Indica que `Proyecto` gestiona la relación.

---

## 5. Persistencia de Datos en Relaciones

### **Ejemplo: Insertar un Departamento con Empleados**

```java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import java.util.ArrayList;
import java.util.List;

public class HibernateMain {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Departamento.class).addAnnotatedClass(Empleado.class).buildSessionFactory();
        Session session = factory.getCurrentSession();
        
        try {
            // Crear Departamento
            Departamento departamento = new Departamento();
            departamento.setNombre("Tecnología");
            
            // Crear Empleados
            Empleado emp1 = new Empleado();
            emp1.setNombre("Carlos");
            emp1.setDepartamento(departamento);
            
            Empleado emp2 = new Empleado();
            emp2.setNombre("Ana");
            emp2.setDepartamento(departamento);
            
            List<Empleado> empleados = new ArrayList<>();
            empleados.add(emp1);
            empleados.add(emp2);
            departamento.setEmpleados(empleados);
            
            // Iniciar transacción
            session.beginTransaction();
            
            // Guardar el departamento (esto también guardará los empleados por cascada)
            session.save(departamento);
            
            // Confirmar transacción
            session.getTransaction().commit();
            
            System.out.println("Datos guardados correctamente.");
        } finally {
            factory.close();
        }
    }
}
```

### **Explicación:**
- Creamos un **departamento** y le asignamos empleados.
- Gracias a `cascade = CascadeType.ALL`, los empleados se guardan automáticamente cuando se guarda el departamento.

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear las entidades `Cliente` y `Pedido`** con una relación **1:N** (un cliente puede tener varios pedidos).
2. **Insertar al menos 2 clientes con pedidos asociados**.
3. **Consultar los pedidos de un cliente y mostrarlos en consola**.

**Ejemplo de salida esperada:**
```
Cliente: Juan Pérez
Pedido 1: Televisor - 500.00€
Pedido 2: Lavadora - 300.00€
Cliente: Ana Gómez
Pedido 1: Smartphone - 900.00€
```

**Pistas:** Usa `@OneToMany`, `@ManyToOne` y `cascade = CascadeType.ALL` para gestionar la relación.

---

Con esta lección, hemos aprendido cómo definir relaciones entre entidades en Hibernate y cómo persistir datos de forma eficiente. En la próxima sesión, exploraremos **consultas avanzadas con Hibernate y HQL**. ¡A programar!
