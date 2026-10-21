# Proyectos descargables

**Al finalizar podrás:** elegir un proyecto completo y ejecutarlo desde terminal o IDE.

Los ZIP incluyen `pom.xml`, código Java comentado, README e instrucciones de ejecución. Descomprime cada archivo y abre **la carpeta que contiene `pom.xml`**.

| Proyecto | Qué enseña | Descargar |
|---|---|---|
| 01 · Hola Maven | Compilar, ejecutar, generar JAR | [01-hola-maven.zip](descargas/01-hola-maven.zip) |
| 02 · Calculadora JUnit | Pruebas automatizadas y Surefire | [02-calculadora-junit.zip](descargas/02-calculadora-junit.zip) |
| 03 · JSON Gson | Dependencias y JAR con Shade | [03-json-gson.zip](descargas/03-json-gson.zip) |

## Comandos para todos los ejemplos

```bash
mvn clean test
mvn package
```

En los tres proyectos la clase principal es `es.dam.maven.App`; puedes ejecutarla con `mvn exec:java`.

## Antes de compartir

No subas la carpeta `target/` ni credenciales. Comprueba las versiones de bibliotecas en Maven Central antes de actualizar el material.

## Guía del profesor

- Proyecto 01: recomendable para instalación, POM y ciclo de vida.
- Proyecto 02: adecuado para JUnit y pruebas que fallan.
- Proyecto 03: útil para dependencias externas y diferencias entre `exec:java` y `java -jar`.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
