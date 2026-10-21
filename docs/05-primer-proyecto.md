# 5. Primer proyecto Maven

**Al finalizar podrás:** crear, compilar y ejecutar un proyecto sencillo.

## Dos maneras de empezar

**Opción 1: descarga el ejemplo** `01-hola-maven.zip` desde [Descargas](descargas.md). Es la más adecuada para aprender primero a leer una estructura.

**Opción 2: genera un proyecto con un arquetipo**:

```bash
mvn archetype:generate -DgroupId=es.dam.maven -DartifactId=hola-maven -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

El arquetipo crea una plantilla inicial. **Revísala**: la versión de Java y las dependencias de prueba que trae por defecto pueden ser antiguas; no des por hecho que el proyecto generado ya usa Java 21 o JUnit 5.

## Coordenadas del proyecto

- `groupId`: organización o espacio de nombres, como `es.dam.maven`.
- `artifactId`: identificador del proyecto, como `hola-maven`.
- `version`: versión del artefacto, como `1.0-SNAPSHOT`.

## Primeras órdenes

```bash
cd hola-maven
mvn validate
mvn compile
mvn test
mvn package
```

Después de `compile`, encontrarás archivos `.class` en `target/classes`. Después de `package`, habitualmente habrá un `.jar` en `target/`.

!!! warning "Un JAR no siempre es ejecutable con java -jar"
    Para ejecutar `java -jar` hace falta que el manifiesto indique la clase principal y que las dependencias estén disponibles. Más adelante configuraremos el empaquetado.

## Ejecutar una clase principal

En el ejemplo descargable puedes usar:

```bash
mvn exec:java -Dexec.mainClass=es.dam.maven.App
```

Maven invoca el objetivo `java` del plugin `exec`; si el plugin no está declarado en el POM, Maven puede resolver una versión según su configuración. En el proyecto descargable ya se declara explícitamente.

## Pregunta

¿Qué diferencia hay entre `mvn compile` y `mvn package`?

??? success "Mostrar solución"
    `compile` llega hasta la fase de compilación de las clases principales. `package` avanza por el ciclo de vida predeterminado, incluyendo compilación y pruebas, y crea el artefacto final según el tipo de empaquetado.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
