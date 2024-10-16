---
title: Manejo de ficheros en Java
author: alberto
categories: [Ficheros]
---

## Introducción

Un **fichero** o **archivo** es un conjunto de **bits** almacenados en un dispositivo. Estos bits contienen **información** que posteriormente puede ser leída por un programa, lo que permite operar con los datos que contiene.

Para operar con los ficheros en java, es decir, crearlos, leerlos, editarlos y borrarlos, exiten una serie de clases que pueden ser de gran utilidad. La primera de ellas es la clase `File`, que permite operar con el fichero en cuestión.

## Clase File

La clase `File` es parte del paquete `java.io`. Esta clase permite operar con los ficheros en cuestión, pero no con su contenido. Adicionalmente, también permite operar con directorios.

Existen tres constructores para esta clase:

- `File(String ruta)`: permite referenciar a un directorio o fichero mediante su ruta absoluta o relativa.
- `File(String directorio, String nombre)`: permite referenciar a un fichero indicando por un lado el directorio en el que se encuentra, y por otro el nombre del fichero.
- `File(File directorio, String nombre)`: similar a la anterior, pero en este caso se pasa el directorio como un objeto de clase `File`.

### Creación

Una vez creado el objeto de clase `File` **no se crea el fichero o directorio automáticamente**, si no que es necesario crearlo usando distintos métodos. En cualquiera de los casos, este **no** se creará si ya existe.

- `boolean mkdir()`: crea un directorio.
- `boolean createNewFile()`: crea un fichero.

Ejemplo:

```java
import java.io.File;

public class Main {
    public static void main(String[] args) throws Exception {
        final String NOMBRE_ARCHIVO = "archivo1.txt";

        File archivo = new File(NOMBRE_ARCHIVO); // Indica cual es el archivo en cuestión.
        archivo.createNewFile(); // Crea el archivo si no existe.
    }
}
```

### Renombrar, mover y eliminar

Para **mover, renombrar o eliminar** archivos referenciados mediante la clase `File` existen varios métodos que pueden ser de gran utilidad.

- `boolean renameTo(File nuevaRuta)`: renombra o mueve el archivo para que coincida con el objeto `File` pasado como parámetro. Debe existir el directorio de destino.
- `boolean delete()`: elimina el archivo o directorio.

Ejemplo:

```java
import java.io.File;

public class Main {
    public static void main(String[] args]) throws Exception {
        final String NOMBRE_ARCHIVO = "archivo1.txt";
        final String NOMBRE_NUEVO = "carpeta/archivo2.txt";

        File archivoOriginal = new File(NOMBRE_ARCHIVO); // Crea el objeto File para el archivo original.
        File archivoNuevo = new File(NOMBRE_NUEVO); // Crea el objeto File para la nueva ruta del archivo.

        archivoOriginal.renameTo(archivoNuevo); // Mueve y renombra el archivo original.
        archivoNuevo.delete(); // Elimina el archivo.
    }
}
```

### Otros métodos de utilidad

A continuación se exponen otros métodos de la clase `File` que pueden ser de utilidad, especialmente para recabar información sobre los ficheros o directorios.

- `String[] list()`: devuelve un array con los nombres de archivos y directorios que contiene el directorio.
- `File[] listFiles()`: similar al anterior, pero devuelve un array de objetos `File` conformada únicamente por fichereos.
- `String getName()`: devuelve el nombre del fichero o directorio.
- `String getPath()`: devuelve la ruta relativa al fichero o directorio.
- `String getAbsoultePath()`: devuelve la ruta absoluta al fichero o directorio.
- `boolean exists()`: devuelve `true` si el fichero o directorio existe.
- `boolean canWrite()`: devuelve `true` si el fichero se puede escribir. 
- `boolean canRead()`: devuelve `true` si el fichero se puede leer. 
- `boolean isFile()`: devuelve `true` si es un fichero.
- `boolean isDirectory()`: devuelve `true` si es un directorio.
- `long lenght()`: devuelve el tamaño del fichero en bytes.
- `String getParent()`: devuelve el nombre del directorio padre, o `null` si no existe.

## Ejercicio práctico

Elabora un programa que, dada una ruta introducida por el usuario en consola:

- Si la ruta no existe muestre un error.
- Si la ruta no es un directorio muestre un error.
- Ordene los archivos en orden alfabético y los renombre como "Archivo 01", "Archivo 02", etc.
- Ordene los directorios en orden alfabético y los renombre como "Directorio 01", "Directorio 02", etc.
- Haga lo mismo con todos los subdirectorios.

**Nota:** En los subdirectorios, el contador de archivos y directorios debe reiniciarse, volviendo a nombrar el primer archivo como "Archivo 01", y el primer directorio como "Directorio 01".

**Nota:** El contador para los ficheros y directorios debe ser distinto, es decir, deben existir al mismo tiempo "Fichero 01" y "Directorio 01".
