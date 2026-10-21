# 7. El archivo pom.xml

**Objetivos:** identificar las coordenadas de un proyecto, entender sus propiedades, configurar Java 21, añadir una dependencia y corregir un proyecto generado con un arquetipo antiguo de VS Code.

!!! info "Antes de empezar"
    Trabajaremos con **JDK 21** y una versión compatible de **Apache Maven 3**. No confundas la versión de Maven, la versión del arquetipo utilizado para crear un proyecto y la versión de Java que queremos utilizar.

## 7.1. ¿Qué es el POM?

POM significa *Project Object Model*. Es el archivo `pom.xml` que Maven busca en la carpeta raíz de un proyecto. Describe su identidad, dependencias y opciones de construcción.

Por ejemplo, en un proyecto llamado `calculadora`:

```text
calculadora/
├── pom.xml
└── src/
    ├── main/java/
    └── test/java/
```

Un POM básico válido es:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>es.dam.maven</groupId>
    <artifactId>calculadora</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```

**¿Qué significan las etiquetas?**

| Etiqueta | Significado | Ejemplo |
|---|---|---|
| `project` | Elemento raíz del POM | Contiene la configuración |
| `modelVersion` | Versión del modelo de POM, no del programa | `4.0.0` |
| `groupId` | Identificador del grupo u organización | `es.dam.maven` |
| `artifactId` | Nombre técnico del proyecto | `calculadora` |
| `version` | Versión del programa | `1.0-SNAPSHOT` |
| `packaging` | Formato del artefacto; si se omite, normalmente `jar` | `jar` |
| `properties` | Configuración y valores reutilizables | Java 21 |
| `dependencies` | Bibliotecas que necesita el proyecto | Gson |
| `build/plugins` | Herramientas de construcción configuradas | Compilador, JAR |

## 7.2. Identificar el proyecto: coordenadas GAV

GAV son las iniciales de **GroupId, ArtifactId y Version**. Identifican el artefacto que Maven construye.

Con el ejemplo anterior:

```text
G: es.dam.maven
A: calculadora
V: 1.0-SNAPSHOT
```

Si ejecutamos `mvn package`, normalmente se genera `target/calculadora-1.0-SNAPSHOT.jar`.

- `SNAPSHOT` suele indicar una versión de desarrollo, todavía modificable.
- Una versión como `1.0.0` suele identificar una versión concreta publicada.

## 7.3. La sección `properties`: configurar Java

```xml
<properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

- **`maven.compiler.release`**: solicita generar código compatible con la versión de Java indicada, teniendo en cuenta sintaxis, bytecode y API del JDK de destino.
- **`project.build.sourceEncoding`**: especifica la codificación de los archivos de texto del proyecto.

Para compilar con `release=21` necesitamos que el compilador seleccionado admita Java 21; en nuestro curso utilizamos JDK 21.

### Dos versiones de Java que conviene distinguir

Cuando ejecutamos:

```bash
mvn -version
```

Maven informa, entre otras cosas, de la versión de Java con la que **se ejecuta Maven**. La propiedad `maven.compiler.release` indica la versión **objetivo del programa que construimos**.

Ejemplo: Maven puede ejecutarse con JDK 21 y compilar una aplicación para Java 17 si configuramos `release=17`. En nuestras prácticas usaremos Java 21 en ambos casos para simplificar.

## 7.4. Problema frecuente en VS Code: Quickstart 1.4 genera Java 7

Al crear un proyecto desde **VS Code → `Maven: Create Maven Project` → `maven-archetype-quickstart` → versión `1.4`**, se utiliza una plantilla antigua. Su `pom.xml` puede contener:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
</properties>
```

**¿Qué significan `1.7` y `1.4`?**

- `1.4` es la **versión del arquetipo** o plantilla de Maven.
- `1.7` es **Java 7**, la versión del lenguaje/bytecode que solicita el proyecto.
- **No son la versión de Apache Maven instalado ni la del JDK del ordenador.**

JDK 21 ya no admite compilar con `source=7` y `target=7`. Por eso puede aparecer un error como:

```text
Source option 7 is no longer supported.
Use 8 or later.
```

### Solución paso a paso

1. Abre el `pom.xml` que ha generado VS Code.
2. Localiza las líneas `maven.compiler.source` y `maven.compiler.target`.
3. **Elimina esas dos líneas** y utiliza `maven.compiler.release`.
4. Guarda el archivo.

La sección corregida queda así:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.release>21</maven.compiler.release>
</properties>
```

5. Abre la terminal integrada de VS Code en la carpeta donde está `pom.xml`.
6. Ejecuta:

```bash
mvn -version
mvn clean compile
```

Si Maven utiliza un JDK compatible y el proyecto no tiene otros errores, la compilación terminará con `BUILD SUCCESS`.

!!! warning "Una segunda comprobación"
    El arquetipo Quickstart 1.4 también puede incluir **plugins o bibliotecas antiguos**. Si al corregir Java aparecen errores de plugins o pruebas, revisa sus versiones. Cambiar `release` soluciona el problema de Java 7, pero no garantiza que toda la configuración antigua sea adecuada para un proyecto moderno.

!!! tip "Si VS Code mantiene errores antiguos"
    Guarda el POM, espera a que se actualice el proyecto Maven y, si hace falta, ejecuta `Java: Clean Java Language Server Workspace` desde `Ctrl + Shift + P`.

## 7.5. Dependencias: añadir una biblioteca externa

Supongamos que queremos utilizar **Gson** para trabajar con JSON. Añadimos esta sección al POM, dentro de `<project>` pero **fuera** de `<properties>`:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.11.0</version>
    </dependency>
</dependencies>
```

Cada `<dependency>` identifica una biblioteca mediante sus coordenadas. Maven intentará descargarla de los repositorios configurados y utilizarla en la construcción del proyecto.

**No confundas:** `dependencies` son bibliotecas para nuestro programa; los `plugins` son herramientas que Maven ejecuta para compilar, probar o empaquetar.

## 7.6. Ejemplo completo: POM básico con Gson y Java 21

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>es.dam.maven</groupId>
    <artifactId>json-basico</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.release>21</maven.compiler.release>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.11.0</version>
        </dependency>
    </dependencies>
</project>
```

Este ejemplo sirve para **compilar** un proyecto que utiliza Gson. Para generar un JAR que incluya también las bibliotecas necesarias para ejecutarlo, estudiaremos el capítulo **12. Empaquetado con Maven**.

## 7.7. Práctica de comprobación

1. Crea un proyecto con `maven-archetype-quickstart:1.4` desde VS Code.
2. Identifica sus valores `groupId`, `artifactId` y `version`.
3. Localiza la versión de Java configurada originalmente.
4. Adapta el POM a Java 21.
5. Ejecuta `mvn clean compile`.
6. Cambia el `artifactId` a `mi-aplicacion`, ejecuta `mvn clean package` y observa el nombre del archivo en `target`.

### Preguntas con solución

**1. ¿Por qué aparece Java 7 si tengo JDK 21 instalado?**

??? success "Mostrar solución"
    Porque el arquetipo 1.4 genera un POM con propiedades antiguas. El JDK instalado no modifica automáticamente las propiedades del proyecto.

**2. ¿Qué líneas debemos sustituir?**

??? success "Mostrar solución"
    Elimina `maven.compiler.source=1.7` y `maven.compiler.target=1.7` y utiliza `<maven.compiler.release>21</maven.compiler.release>`.

**3. ¿Para qué sirve `groupId`?**

??? success "Mostrar solución"
    Identifica el grupo u organización del proyecto. Junto con `artifactId` y `version` forma las coordenadas GAV.

**4. ¿Qué cambia si modificamos `artifactId`?**

??? success "Mostrar solución"
    Cambia la identidad técnica del artefacto y, con la configuración habitual, también el nombre del JAR generado, por ejemplo `mi-aplicacion-1.0-SNAPSHOT.jar`.

**5. ¿Es lo mismo Maven 3.9, Quickstart 1.4 y Java 21?**

??? success "Mostrar solución"
    No. Maven 3.9 es la versión de la herramienta; Quickstart 1.4 es la versión de la plantilla que genera el proyecto; Java 21 es la versión de Java que utilizamos para compilar nuestras prácticas.

## Documentación oficial

- [Referencia del POM](https://maven.apache.org/pom.html)
- [Maven Compiler Plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)
