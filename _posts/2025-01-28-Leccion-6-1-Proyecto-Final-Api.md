---
title: "Proyecto Final: Desarrollo de una API REST Segura con Spring Boot"
author: alberto
date: 2025-01-28 00:00:00
categories: ["Semana 6. Proyecto Final"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Proyecto Final: Desarrollo de una API REST Segura con Spring Boot

## 1. Introducción

En este proyecto final, aplicaremos todo lo aprendido en las últimas sesiones para construir una API REST completa con:
- **Spring Boot** para la estructura del proyecto.
- **Spring Data JPA** para la persistencia de datos.
- **Spring Security y JWT** para la autenticación y seguridad.
- **Pruebas unitarias e integración** para garantizar la calidad del código.

---

## 2. Requisitos del Proyecto

El objetivo es desarrollar una API REST para la gestión de productos y clientes en una tienda en línea. La API debe permitir:

### **1. Autenticación y Autorización**
- Implementar **Spring Security** para proteger los endpoints.
- Utilizar **JWT (JSON Web Token)** para la autenticación de usuarios.
- Crear un sistema de **roles y permisos** para restringir accesos.
- Proporcionar endpoints de **registro e inicio de sesión** para los usuarios.

### **2. Gestión de Productos**
- Crear una entidad **Producto** con los atributos: `id`, `nombre`, `precio`, `stock`.
- Implementar operaciones CRUD:
  - **Crear productos** mediante `POST /productos`.
  - **Obtener la lista de productos** mediante `GET /productos`.
  - **Actualizar un producto** mediante `PUT /productos/{id}`.
  - **Eliminar un producto** mediante `DELETE /productos/{id}`.
- Proteger estos endpoints con autenticación JWT.

### **3. Gestión de Clientes**
- Crear una entidad **Cliente** con los atributos: `id`, `nombre`, `email`, `telefono`.
- Implementar operaciones CRUD:
  - **Registrar clientes** mediante `POST /clientes`.
  - **Obtener la lista de clientes** mediante `GET /clientes`.
  - **Actualizar un cliente** mediante `PUT /clientes/{id}`.
  - **Eliminar un cliente** mediante `DELETE /clientes/{id}`.
- Asegurar que solo usuarios autenticados puedan acceder a esta información.

### **4. Configuración y Persistencia de Datos**
- Configurar **Spring Data JPA** para interactuar con una base de datos relacional (MySQL o PostgreSQL).
- Definir correctamente las relaciones entre entidades en caso de ser necesario.
- Implementar repositorios con **JpaRepository** para acceder a los datos.

### **5. Pruebas y Calidad del Código**
- Escribir **pruebas unitarias** para los servicios con **JUnit y Mockito**.
- Implementar **pruebas de integración** con **Spring Boot Test**.
- Utilizar **MockMvc** para probar los endpoints de la API REST.
- Garantizar un **mínimo del 80% de cobertura de código** en las pruebas.

### **6. Documentación de la API**
- Utilizar **Swagger** o **Springdoc OpenAPI** para documentar la API.
- Asegurar que todos los endpoints y parámetros estén claramente descritos.

---

Este proyecto final consolidará los conocimientos adquiridos en **Spring Boot, JPA, JWT y pruebas automáticas**, asegurando que el desarrollo siga buenas prácticas de arquitectura y seguridad. 🚀 ¡Manos a la obra!
