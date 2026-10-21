# 15. Maven en IntelliJ IDEA

**Objetivos:** crear o abrir proyectos Maven, configurar JDK 21, sincronizar el POM, ejecutar fases y comparar la ejecución desde el IDE con la terminal.

## 15.1. ¿Qué aporta IntelliJ IDEA?

IntelliJ IDEA reconoce proyectos Maven y ofrece una ventana de herramientas para consultar dependencias y ejecutar objetivos del ciclo de vida.

**Recuerda:** el `pom.xml` sigue siendo el archivo de configuración del proyecto, aunque IntelliJ permita realizar muchas operaciones mediante botones y menús.

## 15.2. Abrir un proyecto Maven existente

Utilizaremos `01-hola-maven`.

1. Descarga y descomprime el proyecto.
2. Abre IntelliJ IDEA y selecciona **Open**.
3. Elige la carpeta raíz que contiene `pom.xml` o el propio archivo POM, según la versión del IDE.
4. Confirma que deseas abrir/importar el proyecto como Maven si aparece una pregunta.
5. Espera a que termine la importación y la descarga de dependencias.
6. En la vista del proyecto, localiza `pom.xml`, `src/main/java` y `src/test/java`.

!!! tip "Si IntelliJ pregunta si confías en el proyecto"
    Acepta únicamente si procede de una fuente que conoces. La importación puede ejecutar tareas o resolver componentes externos.

## 15.3. Configurar JDK 21

Abre la configuración del proyecto, normalmente mediante:

```text
File → Project Structure → Project
```

Busca **Project SDK** y selecciona **JDK 21**. Si no aparece, añade la instalación de JDK 21 de tu equipo.

Después abre `pom.xml` y comprueba:

```xml
<properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

**¿Por qué comprobamos ambas cosas?**

- **Project SDK** indica el JDK configurado para el proyecto en IntelliJ.
- **`maven.compiler.release`** indica la versión Java objetivo durante la compilación Maven.

No siempre coinciden automáticamente.

## 15.4. Crear un proyecto Maven desde cero

1. Selecciona **New Project**.
2. Elige **Java** como lenguaje y **Maven** como sistema de construcción, si tu versión presenta estas opciones.
3. Selecciona **JDK 21**.
4. Introduce un nombre, por ejemplo `hola-intellij`.
5. Revisa el `groupId` y el `artifactId` cuando el asistente permita configurarlos.
6. Crea el proyecto.
7. Abre `pom.xml` y confirma que utiliza `release=21`.

!!! note "Los asistentes cambian según la versión"
    Algunas versiones de IntelliJ presentan las opciones con nombres o en pantallas distintas. Lo importante es seleccionar **Maven** como sistema de construcción y comprobar después el POM y el JDK.

## 15.5. Ejecutar una clase Java

Abre `App.java` y localiza el método:

```java
public static void main(String[] args)
```

Haz clic en el icono de ejecución situado junto al método o a la clase y selecciona **Run**. Comprueba el resultado en la ventana de ejecución.

Este procedimiento ejecuta la aplicación desde IntelliJ; no implica necesariamente que hayamos ejecutado todas las fases Maven.

## 15.6. Ejecutar fases Maven desde IntelliJ

Abre la ventana **Maven** (su ubicación y acceso dependen de la versión del IDE). Expande el proyecto y la sección **Lifecycle**.

Encontrarás objetivos como:

- `clean`: limpia resultados anteriores.
- `compile`: compila el código principal.
- `test`: ejecuta pruebas.
- `package`: genera el artefacto.

Ejecuta `package` y localiza el JAR en la carpeta `target`.

Para realizar una construcción limpia también puedes ejecutar desde la terminal integrada:

```bash
mvn clean package
```

**Ventaja didáctica:** así compruebas que el proyecto no depende de los botones de IntelliJ para construirse.

## 15.7. Actualizar las dependencias del POM

Si añadimos una biblioteca al `pom.xml`, IntelliJ suele ofrecer la opción de importar o recargar los cambios. También puedes abrir la ventana Maven y utilizar **Reload All Maven Projects**.

Ejemplo:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.11.0</version>
    </dependency>
</dependencies>
```

**Pasos:**

1. Añade la dependencia en el POM.
2. Guarda el archivo.
3. Recarga el proyecto Maven si IntelliJ no lo sincroniza automáticamente.
4. Espera a que termine la resolución.
5. Comprueba que puedes importar clases de Gson desde Java.

!!! warning "No confundas recargar y compilar"
    **Reload All Maven Projects** actualiza el modelo del proyecto y sus dependencias en IntelliJ. Para comprobar que el código compila y las pruebas pasan, ejecuta además `mvn test` o `mvn package`.

## 15.8. Maven y Java pueden estar configurados en varios lugares

IntelliJ puede utilizar distintas configuraciones para:

| Configuración | Para qué se utiliza |
|---|---|
| **Project SDK** | Desarrollo y ejecución del proyecto en el IDE |
| **Maven home path** | Distribución de Maven utilizada por el IDE |
| **JDK for importer** | Java utilizado durante la importación del proyecto Maven |
| **JRE para Maven Runner** | Java utilizado para ejecutar objetivos Maven desde el IDE |
| **`maven.compiler.release`** | Versión Java objetivo del proyecto |

Estas opciones suelen encontrarse bajo **Settings → Build, Execution, Deployment → Build Tools → Maven**, en apartados como *Importing* y *Runner*. Los nombres exactos pueden variar entre versiones.

Para la terminal integrada, ejecuta:

```bash
mvn -version
```

Compara el resultado con la configuración Maven del IDE cuando observes comportamientos diferentes.

## 15.9. Práctica guiada con pruebas JUnit

Utilizaremos el proyecto descargable `02-calculadora-junit`.

1. Descarga y descomprime el proyecto.
2. Ábrelo en IntelliJ como proyecto Maven.
3. Comprueba JDK 21 y el POM.
4. Busca la clase de pruebas `CalculadoraTest`.
5. Ejecútala desde IntelliJ mediante el icono de ejecución correspondiente.
6. Abre la terminal integrada y ejecuta:

```bash
mvn test
```

7. Compara los resultados. Si ambos pasan, has comprobado el funcionamiento desde el IDE y desde Maven.
8. Modifica temporalmente una aserción para provocar un fallo, ejecuta de nuevo y observa cómo informa cada herramienta. **Restaura después el código original**.

## 15.10. Errores frecuentes

| Problema | Qué revisar |
|---|---|
| IntelliJ no detecta Maven | Abrir carpeta con `pom.xml` y recargar proyecto |
| El proyecto usa Java incorrecto | Project SDK, Maven Runner, JDK for importer y `release` |
| Dependencia en rojo | Guardar POM, recargar Maven, revisar red y repositorios |
| Pruebas funcionan en IntelliJ pero fallan con Maven | Revisar plugins, dependencias, JDK y configuración de pruebas |
| No aparece el JAR | Ejecutar `package` y revisar la consola y `target` |

## 15.11. Preguntas de autoevaluación

**1. ¿Es suficiente con seleccionar JDK 21 como Project SDK?**

??? success "Mostrar solución"
    No siempre. También hay que comprobar la versión Java objetivo del POM y el JDK utilizado para ejecutar Maven, especialmente si hay discrepancias entre el IDE y la terminal.

**2. ¿Qué hace Reload All Maven Projects?**

??? success "Mostrar solución"
    Vuelve a importar y sincronizar la configuración Maven del proyecto. No equivale por sí solo a ejecutar todas las pruebas o generar un JAR.

**3. ¿Por qué ejecutar `mvn test` si ya hemos pulsado Run sobre una prueba?**

??? success "Mostrar solución"
    Para comprobar que las pruebas también funcionan mediante el proceso de construcción Maven, independientemente de la ejecución específica del IDE.

**4. ¿Qué archivo permite abrir el mismo proyecto en VS Code, Eclipse e IntelliJ sin redefinir manualmente sus dependencias?**

??? success "Mostrar solución"
    El archivo `pom.xml`, porque describe la identidad, dependencias y construcción del proyecto Maven.

## Documentación oficial

- [Documentación de Maven en IntelliJ IDEA](https://www.jetbrains.com/help/idea/maven-support.html)
- [Guía de inicio de Maven](https://maven.apache.org/guides/getting-started/)
