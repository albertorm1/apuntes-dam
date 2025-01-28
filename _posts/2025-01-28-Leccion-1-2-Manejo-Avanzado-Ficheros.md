---
title: Manejo Avanzado de Ficheros en Java
author: alberto
date: 2025-01-28 00:00:14
categories: ["Semana 1. Acceso a Ficheros y XML"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Manejo Avanzado de Ficheros en Java

## 1. Introducción

En la lección anterior aprendimos a leer y escribir archivos de texto en Java usando `FileReader` y `FileWriter`. Hoy profundizaremos en técnicas más avanzadas para trabajar con archivos, incluyendo:

- Lectura y escritura eficiente con `BufferedReader` y `BufferedWriter`.
- Manejo de archivos CSV.
- Manejo de excepciones en operaciones de archivos.

## 2. Lectura y Escritura con `BufferedReader` y `BufferedWriter`

### **Lectura de Archivos con `BufferedReader`**

El uso de `BufferedReader` mejora la eficiencia al leer archivos, ya que permite leer líneas completas en lugar de carácter por carácter.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReadExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("archivo.txt"))) {
            String linea;
            while ((linea = reader.readLine()) != null) {
                System.out.println(linea);
            }
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

### **Escritura en Archivos con `BufferedWriter`**

El `BufferedWriter` permite escribir en archivos de manera eficiente al manejar buffers internos.

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriteExample {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("archivo.txt", true))) {
            writer.write("Esta es una nueva línea de texto.\n");
            writer.write("Otra línea de información.\n");
            System.out.println("Texto añadido correctamente.");
        } catch (IOException e) {
            System.out.println("Error al escribir en el archivo: " + e.getMessage());
        }
    }
}
```

### **Diferencias Clave entre `FileReader/FileWriter` y `BufferedReader/BufferedWriter`**
| Característica    | `FileReader` / `FileWriter` | `BufferedReader` / `BufferedWriter` |
| ----------------- | --------------------------- | ----------------------------------- |
| Lectura/Escritura | Carácter por carácter       | Línea por línea (más rápido)        |
| Eficiencia        | Baja                        | Alta (usa buffers internos)         |
| Uso recomendado   | Archivos pequeños           | Archivos grandes                    |

---

## 3. Manejo de Archivos CSV en Java

Los archivos **CSV** (Comma-Separated Values) son muy utilizados para almacenar datos en formato estructurado. En Java, podemos leer y escribir archivos CSV usando `BufferedReader` y `BufferedWriter`.

### **Lectura de un Archivo CSV**

Supongamos que tenemos un archivo `datos.csv` con el siguiente contenido:
```
ID,Nombre,Edad
1,Juan,25
2,Ana,30
3,Pedro,28
```
Podemos leerlo línea por línea y dividir los valores por comas:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadCSVExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("datos.csv"))) {
            String linea;
            while ((linea = reader.readLine()) != null) {
                String[] datos = linea.split(",");
                System.out.println("ID: " + datos[0] + ", Nombre: " + datos[1] + ", Edad: " + datos[2]);
            }
        } catch (IOException e) {
            System.out.println("Error al leer el archivo CSV: " + e.getMessage());
        }
    }
}
```

### **Escritura de un Archivo CSV**

Podemos escribir en un archivo CSV usando `BufferedWriter` de la siguiente manera:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class WriteCSVExample {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("datos.csv", true))) {
            writer.write("4,María,22\n");
            writer.write("5,Lucas,29\n");
            System.out.println("Datos añadidos al archivo CSV.");
        } catch (IOException e) {
            System.out.println("Error al escribir en el archivo CSV: " + e.getMessage());
        }
    }
}
```

---

## 4. Manejo de Excepciones en Archivos

Trabajar con archivos puede generar errores, por lo que es importante manejar excepciones correctamente.

- `FileNotFoundException`: Ocurre si el archivo no existe.
- `IOException`: Ocurre si hay problemas al leer o escribir.
- `SecurityException`: Ocurre si no se tienen permisos para acceder al archivo.

Ejemplo de manejo de errores:

```java
import java.io.*;

public class ExceptionHandlingExample {
    public static void main(String[] args) {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("archivo_inexistente.txt"));
            System.out.println(reader.readLine());
            reader.close();
        } catch (FileNotFoundException e) {
            System.out.println("Error: El archivo no fue encontrado.");
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

---

## 5. **Ejercicio Práctico**

### **Objetivo:**
Crea un programa en Java que haga lo siguiente:
1. **Genere un archivo CSV** llamado `empleados.csv`.
2. **Escriba** en el archivo al menos 3 registros con `ID, Nombre, Cargo, Salario`.
3. **Lea el archivo CSV** e imprima los datos en la consola.

**Ejemplo de contenido esperado en `empleados.csv`**:
```
ID,Nombre,Cargo,Salario
1,Ana,Desarrolladora,3500
2,Juan,Analista,3200
3,María,Gerente,5000
```

**Pistas:** Usa `BufferedWriter` para escribir y `BufferedReader` para leer. Maneja posibles excepciones correctamente.

---

Con esto hemos aprendido cómo manejar archivos de texto y CSV de manera eficiente en Java. En la próxima clase, veremos cómo trabajar con archivos XML usando `DOM` y `SAX`. ¡A programar!
