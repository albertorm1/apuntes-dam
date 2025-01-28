---
title: Introducción a XML y DOM en Java 
author: alberto
date: 2025-01-28 00:00:13
categories: ["Semana 1. Acceso a Ficheros y XML"]
---

> Este contenido ha sido generado con **Inteligencia Artificial**. Es posible que contenga errores y no se ajuste a las
> buenas prácticas de programación requeridas por los profesores.
{: .prompt-warning}

# Introducción a XML y DOM en Java

## 1. Introducción

XML (**Extensible Markup Language**) es un formato de almacenamiento de datos estructurado ampliamente utilizado para la interoperabilidad entre sistemas. Java proporciona varias formas de manipular archivos XML, siendo **DOM (Document Object Model)** una de las más comunes.

En esta lección aprenderemos:
- Qué es XML y su estructura.
- Cómo leer archivos XML en Java usando **DOM**.
- Cómo escribir archivos XML con **DOM**.

## 2. Conceptos Básicos de XML

Un archivo XML sigue una estructura jerárquica de etiquetas, similar a HTML. Ejemplo de un archivo `empleados.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<empleados>
    <empleado>
        <id>1</id>
        <nombre>Ana</nombre>
        <cargo>Desarrolladora</cargo>
        <salario>3500</salario>
    </empleado>
    <empleado>
        <id>2</id>
        <nombre>Juan</nombre>
        <cargo>Analista</cargo>
        <salario>3200</salario>
    </empleado>
</empleados>
```

---

## 3. Lectura de XML con DOM en Java

### **Pasos para leer un XML con DOM**
1. Cargar el archivo XML.
2. Convertirlo en un documento DOM.
3. Recorrer los nodos y extraer la información.

```java
import org.w3c.dom.*;
import javax.xml.parsers.*;
import java.io.File;

public class ReadXML {
    public static void main(String[] args) {
        try {
            File file = new File("empleados.xml");
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document document = builder.parse(file);
            document.getDocumentElement().normalize();

            NodeList nodeList = document.getElementsByTagName("empleado");
            
            for (int i = 0; i < nodeList.getLength(); i++) {
                Node node = nodeList.item(i);
                if (node.getNodeType() == Node.ELEMENT_NODE) {
                    Element element = (Element) node;
                    System.out.println("ID: " + element.getElementsByTagName("id").item(0).getTextContent());
                    System.out.println("Nombre: " + element.getElementsByTagName("nombre").item(0).getTextContent());
                    System.out.println("Cargo: " + element.getElementsByTagName("cargo").item(0).getTextContent());
                    System.out.println("Salario: " + element.getElementsByTagName("salario").item(0).getTextContent());
                    System.out.println("---------------------");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### **Explicación:**
- Se usa `DocumentBuilderFactory` y `DocumentBuilder` para leer el XML.
- `getElementsByTagName("empleado")` obtiene todos los nodos `<empleado>`.
- Se recorren los nodos y se extrae la información con `getElementsByTagName().item(0).getTextContent()`.

---

## 4. Escritura de XML con DOM en Java

### **Pasos para escribir un XML con DOM**
1. Crear un nuevo documento XML.
2. Agregar elementos y atributos.
3. Guardar el XML en un archivo.

```java
import org.w3c.dom.*;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;

public class WriteXML {
    public static void main(String[] args) {
        try {
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document document = builder.newDocument();
            
            Element root = document.createElement("empleados");
            document.appendChild(root);
            
            Element empleado = document.createElement("empleado");
            root.appendChild(empleado);
            
            Element id = document.createElement("id");
            id.appendChild(document.createTextNode("3"));
            empleado.appendChild(id);
            
            Element nombre = document.createElement("nombre");
            nombre.appendChild(document.createTextNode("María"));
            empleado.appendChild(nombre);
            
            Element cargo = document.createElement("cargo");
            cargo.appendChild(document.createTextNode("Gerente"));
            empleado.appendChild(cargo);
            
            Element salario = document.createElement("salario");
            salario.appendChild(document.createTextNode("5000"));
            empleado.appendChild(salario);
            
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            transformer.setOutputProperty(OutputKeys.INDENT, "yes");
            DOMSource source = new DOMSource(document);
            StreamResult result = new StreamResult(new File("empleados.xml"));
            transformer.transform(source, result);
            
            System.out.println("Archivo XML generado correctamente.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### **Explicación:**
- Se crea un documento XML con `DocumentBuilder`.
- Se agregan nodos (`Element`) y se les asigna contenido con `appendChild()`.
- Se usa `Transformer` para guardar el XML en un archivo.

---

## 5. **Ejercicio Práctico**

### **Objetivo:**
Crear un programa en Java que haga lo siguiente:
1. **Genere un archivo XML** llamado `productos.xml` con información de 3 productos.
2. **Lea el archivo XML** e imprima los datos en consola.

**Ejemplo de contenido esperado en `productos.xml`**:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<productos>
    <producto>
        <id>1</id>
        <nombre>Teclado</nombre>
        <precio>50.00</precio>
    </producto>
    <producto>
        <id>2</id>
        <nombre>Monitor</nombre>
        <precio>200.00</precio>
    </producto>
</productos>
```

**Pistas:** Usa `DocumentBuilderFactory` y `TransformerFactory` para escribir y `DocumentBuilder` para leer el XML.

---

Con esta lección, hemos aprendido a manipular archivos XML en Java usando DOM. En la próxima clase, comenzaremos con el manejo de bases de datos relacionales usando JDBC. ¡A programar!
