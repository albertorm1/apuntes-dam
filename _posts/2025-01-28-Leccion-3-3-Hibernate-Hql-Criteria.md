---
title: Consultas con Hibernate - HQL y Criteria API
author: alberto
date: 2025-01-28 00:00:07
categories: ["Semana 3. ORM con Hibernate"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Consultas con Hibernate - HQL y Criteria API

## 1. Introducción

En las sesiones anteriores, aprendimos a modelar entidades y relaciones en Hibernate. Hoy nos enfocaremos en cómo realizar consultas avanzadas utilizando **Hibernate Query Language (HQL)** y **Criteria API**.

En esta lección aprenderemos:
- Qué es HQL y cómo se diferencia de SQL.
- Cómo realizar consultas con HQL.
- Cómo usar Criteria API para consultas dinámicas.
- Comparación entre HQL y Criteria API.

---

## 2. Introducción a HQL (Hibernate Query Language)

### **¿Qué es HQL?**
HQL es un lenguaje de consulta orientado a objetos que permite recuperar datos en Hibernate utilizando nombres de entidades en lugar de nombres de tablas.

### **Diferencias entre HQL y SQL**
| Característica          | HQL                            | SQL                          |
| ----------------------- | ------------------------------ | ---------------------------- |
| Basado en               | Clases y objetos de Java       | Tablas y columnas            |
| Portabilidad            | Independiente de la BD         | Dependiente de la BD         |
| Soporte para relaciones | Sí, usa asociaciones y objetos | No, se usan joins explícitos |

### **Ejemplo: Consulta HQL para Obtener Todos los Empleados**

```java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import java.util.List;

public class ConsultaHQL {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Empleado.class).buildSessionFactory();
        Session session = factory.getCurrentSession();
        
        try {
            session.beginTransaction();
            
            List<Empleado> empleados = session.createQuery("FROM Empleado", Empleado.class).getResultList();
            
            for (Empleado emp : empleados) {
                System.out.println(emp.getNombre() + " - " + emp.getCargo());
            }
            
            session.getTransaction().commit();
        } finally {
            factory.close();
        }
    }
}
```

### **Explicación:**
- `FROM Empleado` selecciona todos los empleados de la tabla sin usar `SELECT *`.
- La consulta usa nombres de clase en lugar de nombres de tabla.
- `session.createQuery(query, Clase.class).getResultList()` ejecuta la consulta y devuelve la lista de resultados.

---

## 3. Consultas con Parámetros en HQL

Para evitar inyecciones SQL y mejorar la eficiencia, podemos utilizar parámetros en HQL:

### **Ejemplo: Consultar Empleados por Cargo**

```java
String hql = "FROM Empleado WHERE cargo = :cargo";
List<Empleado> empleados = session.createQuery(hql, Empleado.class)
                                 .setParameter("cargo", "Desarrollador")
                                 .getResultList();
```

### **Explicación:**
- `:cargo` es un **parámetro nombrado**.
- `setParameter("cargo", "Desarrollador")` asigna el valor al parámetro.

---

## 4. Uso de Criteria API

### **¿Qué es Criteria API?**
Criteria API permite construir consultas dinámicas de forma programática, sin necesidad de escribir HQL o SQL directamente.

### **Ejemplo: Obtener Todos los Empleados con Criteria API**

```java
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
import jakarta.persistence.criteria.CriteriaBuilder;
import jakarta.persistence.criteria.CriteriaQuery;
import jakarta.persistence.criteria.Root;
import java.util.List;

public class ConsultaCriteria {
    public static void main(String[] args) {
        SessionFactory factory = new Configuration().configure("hibernate.cfg.xml").addAnnotatedClass(Empleado.class).buildSessionFactory();
        Session session = factory.getCurrentSession();

        try {
            session.beginTransaction();
            
            CriteriaBuilder builder = session.getCriteriaBuilder();
            CriteriaQuery<Empleado> query = builder.createQuery(Empleado.class);
            Root<Empleado> root = query.from(Empleado.class);
            query.select(root);
            
            List<Empleado> empleados = session.createQuery(query).getResultList();
            
            for (Empleado emp : empleados) {
                System.out.println(emp.getNombre() + " - " + emp.getCargo());
            }
            
            session.getTransaction().commit();
        } finally {
            factory.close();
        }
    }
}
```

### **Explicación:**
- `CriteriaBuilder` se usa para construir la consulta de manera programática.
- `CriteriaQuery<Empleado>` define el tipo de resultado.
- `Root<Empleado>` representa la tabla `Empleado`.

---

## 5. Comparación entre HQL y Criteria API

| Característica      | HQL                                       | Criteria API                   |
| ------------------- | ----------------------------------------- | ------------------------------ |
| Legibilidad         | Más fácil de leer y escribir              | Más flexible pero más verboso  |
| Consultas dinámicas | No tan eficiente para consultas dinámicas | Ideal para consultas dinámicas |
| Portabilidad        | Alta                                      | Alta                           |

---

## 6. **Ejercicio Práctico**

### **Objetivo:**
1. **Crear una consulta HQL** para obtener todos los empleados con salario mayor a 3000.
2. **Crear una consulta con Criteria API** para obtener los empleados de un departamento específico.

**Ejemplo de salida esperada:**
```
Empleados con salario mayor a 3000:
Juan - 3500
Ana - 4000

Empleados del departamento de TI:
Carlos - Desarrollador
María - Analista
```

**Pistas:**
- Usa `WHERE salario > :salario` en HQL.
- Usa `builder.equal(root.get("departamento"), "TI")` en Criteria API.

---

Con esta lección, hemos aprendido cómo realizar consultas avanzadas en Hibernate usando HQL y Criteria API. En la próxima clase, comenzaremos con bases de datos NoSQL y MongoDB. ¡A programar!
