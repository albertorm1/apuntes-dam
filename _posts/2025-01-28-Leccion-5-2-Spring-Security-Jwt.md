---
title: Seguridad y Autenticación en una API REST con Spring Boot y JWT
author: alberto
date: 2025-01-28 00:00:02
categories: ["Semana 5. API REST"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Seguridad y Autenticación en una API REST con Spring Boot y JWT

## 1. Introducción

Para proteger una API REST, es fundamental implementar autenticación y autorización. En esta lección aprenderemos:
- Qué es **JWT (JSON Web Token)** y cómo funciona.
- Cómo agregar **Spring Security** a nuestra API REST.
- Cómo implementar autenticación con JWT en **Spring Boot**.

---

## 2. ¿Qué es JWT y Cómo Funciona?

JWT (**JSON Web Token**) es un estándar para autenticación basado en tokens. Un **token JWT** se compone de tres partes:
1. **Header**: Información sobre el algoritmo usado.
2. **Payload**: Contiene los datos del usuario y sus permisos.
3. **Signature**: Verifica la autenticidad del token.

### **Ejemplo de un Token JWT**
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.eyJzdWIiOiJ1c3VhcmlvIiwicm9sZXMiOlsiVVNFUiJdLCJleHAiOjE2NzMyNzA1MDB9
.4f7s8D_xKpH0Rbyv4HZ3X-4ERBuMTvBhXz0TnYt9oOc
```

### **Flujo de Autenticación con JWT**
1. El usuario envía credenciales (`username` y `password`).
2. Si las credenciales son correctas, el servidor genera un **JWT** y lo devuelve.
3. En cada solicitud posterior, el cliente envía el token en la cabecera `Authorization`.
4. El servidor valida el token antes de permitir el acceso a los recursos protegidos.

---

## 3. Configuración del Proyecto con Spring Security y JWT

### **Dependencias en `pom.xml`**

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.11.2</version>
    </dependency>
</dependencies>
```

---

## 4. Creación del Modelo `Usuario`

Definimos la entidad que representa a los usuarios en la base de datos.

```java
import jakarta.persistence.*;
import java.util.Set;

@Entity
@Table(name = "usuarios")
public class Usuario {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
    private String password;
    private String rol;

    // Getters y Setters
}
```

---

## 5. Creación del Servicio de Usuario y Autenticación

### **Servicio para Manejo de Usuarios**
```java
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;
import java.util.Collections;

@Service
public class UsuarioService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        if (username.equals("admin")) {
            return new User("admin", "{noop}password", Collections.emptyList());
        }
        throw new UsernameNotFoundException("Usuario no encontrado");
    }
}
```

### **Explicación:**
- `UserDetailsService` se usa para obtener los datos del usuario desde la base de datos.
- `{noop}password` indica que la contraseña no está encriptada (solo para pruebas).

---

## 6. Creación del Filtro para Validar Tokens JWT

```java
import io.jsonwebtoken.*;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.filter.OncePerRequestFilter;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class JwtTokenFilter extends OncePerRequestFilter {
    private final String SECRET_KEY = "mi_clave_secreta";

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String token = request.getHeader("Authorization");
        if (token != null && token.startsWith("Bearer ")) {
            token = token.replace("Bearer ", "");
            try {
                Claims claims = Jwts.parser().setSigningKey(SECRET_KEY).parseClaimsJws(token).getBody();
                UsernamePasswordAuthenticationToken auth = new UsernamePasswordAuthenticationToken(claims.getSubject(), null, Collections.emptyList());
                SecurityContextHolder.getContext().setAuthentication(auth);
            } catch (JwtException e) {
                response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
                return;
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

---

## 7. Creación del Controlador de Autenticación

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.web.bind.annotation.*;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/auth")
public class AuthController {
    private final String SECRET_KEY = "mi_clave_secreta";

    @PostMapping("/login")
    public Map<String, String> login(@RequestParam String username, @RequestParam String password) {
        String token = Jwts.builder()
                .setSubject(username)
                .setExpiration(new Date(System.currentTimeMillis() + 86400000))
                .signWith(SignatureAlgorithm.HS256, SECRET_KEY)
                .compact();
        
        Map<String, String> response = new HashMap<>();
        response.put("token", token);
        return response;
    }
}
```

---

## 8. **Ejercicio Práctico**

### **Objetivo:**
1. **Agregar autenticación con JWT a la API de productos.**
2. **Proteger el endpoint `GET /productos` para que solo sea accesible con un token JWT válido.**
3. **Implementar un nuevo endpoint `POST /auth/login` que genere un token JWT.**

**Ejemplo de salida esperada:**
```
POST /auth/login
{
    "username": "admin",
    "password": "password"
}
Respuesta:
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

GET /productos (con token en la cabecera Authorization)
Respuesta:
[
    {"id":1, "nombre":"Laptop", "precio":1200.00, "stock":10}
]
```

**Pistas:**
- Usa `SecurityContextHolder` para validar tokens.
- Implementa un `JwtTokenFilter` como filtro de seguridad.

---

Con esta lección, hemos aprendido cómo agregar **seguridad con JWT** en una API REST con **Spring Boot**. En la próxima sesión, implementaremos **pruebas automatizadas para nuestra API REST**. ¡A programar!
