# 12. Empaquetado con Maven

En los capítulos anteriores hemos utilizado `mvn compile` y `mvn test`. Ahora nos preguntamos: **¿qué entrega Maven cuando terminamos de construir el proyecto?** La respuesta depende del tipo de empaquetado, indicado por `packaging` en el `pom.xml`. En este capítulo nos centraremos en el caso más habitual de las prácticas: un proyecto Java que genera un JAR.

**El objetivo no es aprender a distribuir aplicaciones Java de todas las formas posibles**, sino comprender qué hace Maven durante la fase `package`, qué plugins intervienen y cómo inspeccionar el resultado.

**Objetivos:** comprender qué produce `mvn package`, distinguir un JAR normal de uno ejecutable y aprender a incluir dependencias externas cuando sea necesario.

## 12.1. ¿Qué es empaquetar?

En un proyecto Java de consola normalmente encontramos `<packaging>jar</packaging>` o no indicamos ningún `packaging`, en cuyo caso Maven utiliza `jar` por defecto. Para otros tipos de proyecto existen formatos como `war`, pero no los estudiaremos aquí.

Durante `mvn package`, Maven llega a la fase de empaquetado después de ejecutar las fases previas necesarias. El **Maven JAR Plugin** es el encargado de crear el JAR en un proyecto de tipo `jar`. La fase y el plugin no son lo mismo: la fase indica **cuándo**, y el plugin aporta **la tarea**.


Durante el desarrollo tenemos archivos `.java`, recursos y bibliotecas. Para entregar nuestro programa suele ser útil reunir el código compilado en un **artefacto**. En proyectos Java sencillos, Maven suele crear un archivo **JAR** (*Java ARchive*).

```text
Proyecto Java
   ├── src/main/java       → clases fuente
   ├── src/main/resources  → recursos
   └── pom.xml             → configuración
              │
              ▼ mvn package
        target/miapp-1.0-SNAPSHOT.jar
```

Un JAR es un archivo ZIP con una estructura definida. Puede contener clases `.class`, recursos y un directorio `META-INF`.

### Paso 1. Compilar y empaquetar

Abre una terminal en la carpeta donde está `pom.xml`:

```bash
mvn clean package
```

- `clean` elimina los resultados anteriores de `target`.
- `package` ejecuta las fases necesarias hasta empaquetar, incluidas compilación y pruebas cuando están configuradas.
- El resultado habitual se guarda en `target/`.

Si utilizas el proyecto descargable `01-hola-maven`, encontrarás un archivo parecido a:

```text
target/hola-maven-1.0-SNAPSHOT.jar
```

!!! note "Importante"
    El nombre exacto depende de `artifactId`, `version` y de la configuración de empaquetado. Comprueba el contenido real de tu carpeta `target` antes de ejecutar el JAR.

## 12.2. Tres tipos de JAR que debemos distinguir

| Tipo | Qué contiene | Cómo se ejecuta |
|---|---|---|
| **JAR normal** | Clases y recursos de nuestro proyecto | Se usa como biblioteca o se ejecuta indicando clase y classpath si procede |
| **JAR ejecutable** | Clases y un manifiesto que indica la clase principal | `java -jar archivo.jar`, siempre que tenga acceso a las dependencias necesarias |
| **JAR con dependencias** (*fat/uber JAR*) | Nuestro código y bibliotecas externas empaquetadas juntas | Normalmente `java -jar archivo.jar` si está configurado correctamente |

**Un JAR ejecutable no tiene por qué contener las dependencias externas.** Esta es la distinción fundamental.

## 12.3. La clase principal: `main`

Para iniciar una aplicación Java necesitamos un punto de entrada. Por ejemplo, en `src/main/java/es/dam/maven/App.java`:

```java
package es.dam.maven;

public class App {
    public static void main(String[] args) {
        System.out.println("¡Hola desde Maven!");
    }
}
```

El nombre completo de la clase es `es.dam.maven.App` porque combina el paquete y el nombre de la clase.

## 12.4. ¿Qué es el manifiesto `MANIFEST.MF`?

Dentro del JAR puede existir el archivo `META-INF/MANIFEST.MF`. Para ejecutar un JAR con `java -jar`, el manifiesto debe indicar una clase principal válida mediante `Main-Class`.

Ejemplo conceptual de contenido:

```text
Manifest-Version: 1.0
Main-Class: es.dam.maven.App
```

No necesitamos escribir ese archivo manualmente: podemos pedirle a Maven que lo genere.

### Paso 2. Configurar Maven JAR Plugin

En `pom.xml`, añade **dentro de `<project>`**, al mismo nivel que `<properties>`, esta sección:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.4.2</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>es.dam.maven.App</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**¿Qué hace cada elemento?**

- `<build>` contiene opciones de construcción.
- `<plugins>` agrupa herramientas que Maven utiliza.
- `maven-jar-plugin` construye el JAR.
- `<mainClass>` indica qué clase debe arrancar Java.

!!! warning "No dupliques `<build>`"
    Si tu POM ya contiene una sección `<build>`, **no crees otra**. Añade el plugin a su `<plugins>` existente. El proyecto descargable `01-hola-maven` ya incorpora esta configuración, por lo que no necesitas repetirla.

### Paso 3. Generar y ejecutar

```bash
mvn clean package
java -jar target/hola-maven-1.0-SNAPSHOT.jar
```

Salida esperada para el ejemplo:

```text
¡Hola desde Maven!
```

Si tu proyecto se llama de otra forma, sustituye el nombre del JAR por el que se haya generado realmente.

## 12.5. ¿Qué ocurre si usamos una biblioteca externa?

Imagina que `App` utiliza Gson:

```java
import com.google.gson.Gson;
```

Maven puede compilar el proyecto porque resuelve la dependencia durante la construcción. Sin embargo, el **JAR normal no suele incluir el código de Gson**. Si lo ejecutamos aisladamente con `java -jar`, podría aparecer:

```text
java.lang.NoClassDefFoundError: com/google/gson/Gson
```

El error significa que Java no encuentra una clase necesaria **durante la ejecución**.

**¿Por qué funciona en el IDE y falla fuera?** Porque el IDE y los plugins de ejecución pueden construir un *classpath* con las dependencias; `java -jar` no las incorpora automáticamente.

## 12.6. Crear un JAR con dependencias usando Maven Shade Plugin

Para distribuir una aplicación pequeña en un solo archivo, podemos utilizar **Maven Shade Plugin**. Este plugin combina el contenido del proyecto y de sus dependencias en un JAR.

Ejemplo de configuración:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.6.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>es.dam.maven.App</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

**Interpretación:**

1. Maven llega a la fase `package`.
2. Ejecuta el objetivo `shade`.
3. Incorpora las dependencias correspondientes al JAR combinado.
4. Configura la clase principal en el manifiesto.

!!! tip "Proyecto de ejemplo"
    El descargable **`03-json-gson`** del curso ya incluye una configuración de Shade. Puedes utilizarlo sin escribir inicialmente toda la configuración anterior.

### Paso 4. Probar el proyecto con Gson

1. Descarga y descomprime `03-json-gson` desde la sección **Descargas**.
2. Abre la carpeta del proyecto, donde está su `pom.xml`.
3. Ejecuta `mvn clean package`.
4. Examina los archivos generados en `target`.
5. Ejecuta el JAR principal con `java -jar` usando **su nombre real**.
6. Comprueba que funciona fuera del IDE.

El plugin puede generar también un archivo de respaldo con el JAR original; no confundas ese archivo con el JAR combinado.

## 12.7. ¿Cómo se entrega una aplicación?

Para una práctica, puedes entregar el código fuente y el `pom.xml`, o el artefacto generado si así se solicita. En una distribución real debes valorar:

- **Versión de Java requerida** en el equipo de destino (nuestro curso: Java 21).
- **Dependencias** incluidas o distribuidas aparte.
- **Archivos externos** de configuración, si existen.
- **Licencias** de bibliotecas de terceros.
- **Instrucciones de ejecución**, por ejemplo en un `README.md`.

Un JAR no incluye necesariamente una máquina virtual Java: el destinatario suele necesitar una versión compatible de Java instalada.

## 12.8. Práctica guiada

**Parte A.** Utiliza `01-hola-maven`:

1. Ejecuta `mvn clean package`.
2. Localiza el JAR.
3. Ejecútalo con `java -jar`.
4. Cambia el mensaje de `App`, vuelve a empaquetar y comprueba el resultado.

**Parte B.** Utiliza `03-json-gson`:

1. Localiza la dependencia Gson en el POM.
2. Busca `maven-shade-plugin`.
3. Ejecuta `mvn clean package`.
4. Comprueba que el JAR resultante funciona desde una terminal.

### Autoevaluación

**1. ¿Qué diferencia existe entre `compile` y `package`?**

??? success "Mostrar solución"
    `compile` compila las clases principales. `package` avanza por el ciclo de vida hasta generar el artefacto distribuible y ejecuta las fases anteriores necesarias.

**2. ¿Es suficiente con incluir `Main-Class` para ejecutar una aplicación que utiliza Gson?**

??? success "Mostrar solución"
    No necesariamente. `Main-Class` identifica el punto de entrada, pero la aplicación también debe encontrar Gson durante la ejecución. Podemos proporcionar la biblioteca en el classpath o generar un JAR con dependencias.

**3. ¿Qué hace Maven Shade Plugin?**

??? success "Mostrar solución"
    Permite combinar el código de la aplicación y las dependencias correspondientes en un JAR, facilitando su distribución. También puede configurar el manifiesto para hacerlo ejecutable.

**4. ¿Por qué podría fallar un JAR en un ordenador sin Java?**

??? success "Mostrar solución"
    Porque el JAR contiene código Java compilado, pero no necesariamente incluye el entorno de ejecución Java. El equipo necesita una JVM compatible o una distribución que incorpore un runtime.

## Documentación oficial

- [Maven JAR Plugin](https://maven.apache.org/plugins/maven-jar-plugin/)
- [Maven Shade Plugin](https://maven.apache.org/plugins/maven-shade-plugin/)
- [Ciclo de vida de Maven](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
