---
title: Introducción a Ficheros en Java
author: alberto
date: 2025-01-28 00:00:15
categories: ["Semana 1. Acceso a Ficheros y XML"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Introducción a Ficheros en Java

## 1. Introducción

El acceso a ficheros en Java es una parte fundamental de la gestión de datos en aplicaciones. Java proporciona diversas clases en el paquete `java.io` para leer y escribir archivos.

En esta lección aprenderemos:
- Qué es un fichero y cómo se maneja en Java.
- Lectura y escritura en archivos de texto.
- Uso de `File`, `FileReader`, y `FileWriter`.

## 2. La Clase `File`

La clase `File` nos permite manipular archivos y directorios sin necesidad de abrirlos. Algunos métodos útiles son:

```java
import java.io.File;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("archivo.txt");
        
        if (file.exists()) {
            System.out.println("El archivo existe.");
        } else {
            System.out.println("El archivo no existe.");
        }
    }
}
```

### Métodos importantes de `File`:
- `exists()`: Comprueba si el archivo o directorio existe.
- `getName()`: Obtiene el nombre del archivo.
- `getAbsolutePath()`: Obtiene la ruta absoluta.
- `length()`: Obtiene el tamaño del archivo en bytes.
- `delete()`: Elimina el archivo.

## 3. Escritura en Archivos con `FileWriter`

Para escribir en un archivo de texto usamos la clase `FileWriter`:

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteFileExample {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("archivo.txt");
            writer.write("Hola, este es un ejemplo de escritura en archivos.\n");
            writer.close();
            System.out.println("Archivo escrito correctamente.");
        } catch (IOException e) {
            System.out.println("Ocurrió un error: " + e.getMessage());
        }
    }
}
```

### Notas:
- `write(String texto)`: Escribe texto en el archivo.
- `close()`: Cierra el flujo de escritura (importante para evitar pérdida de datos).
- `FileWriter` sobrescribe el contenido si el archivo ya existe. Para añadir texto, se usa `new FileWriter("archivo.txt", true)`.

## 4. Lectura de Archivos con `FileReader`

Para leer archivos usamos la clase `FileReader`:

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadFileExample {
    public static void main(String[] args) {
        try {
            FileReader reader = new FileReader("archivo.txt");
            int caracter;
            while ((caracter = reader.read()) != -1) {
                System.out.print((char) caracter);
            }
            reader.close();
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
```

### Notas:
- `read()`: Devuelve el siguiente carácter como un número entero, o `-1` si es el final del archivo.
- `close()`: Cierra el flujo de lectura.

## 5. Ejercicio Práctico

### **Objetivo:**
Crear un programa en Java que haga lo siguiente:
1. Cree un archivo `notas.txt`.
2. Escriba al menos 3 líneas de texto en el archivo.
3. Lea el contenido del archivo y lo imprima en la consola.

**Pistas:** Usa `FileWriter` para escribir y `FileReader` para leer.

---

Con esto hemos aprendido los conceptos básicos sobre el manejo de archivos en Java. En la próxima sesión, veremos cómo trabajar con archivos de mayor tamaño usando `BufferedReader` y `BufferedWriter`. ¡A programar!
