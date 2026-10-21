# 11. Plugins de Maven

Cuando comenzamos a utilizar Maven, escribimos órdenes como `mvn compile`, `mvn test` o `mvn package` y obtenemos resultados sin indicar cómo se compila cada clase o cómo se crea un JAR. Esto puede dar la impresión de que Maven incorpora todas esas funciones directamente. En realidad, **Maven coordina el proceso y los plugins realizan muchas de las tareas concretas**.

En este capítulo aprenderemos a reconocerlos y a configurar uno de ellos. No vamos a desarrollar plugins propios: nos interesa comprender los que se utilizan habitualmente en proyectos Java.

**Al finalizar podrás:** explicar qué es un plugin, distinguir *fase* y *goal*, localizar plugins en el `pom.xml`, configurar un plugin y diagnosticar errores básicos.

## 11.1. Qué es un plugin y por qué existe

Un **plugin de Maven** es un componente que añade tareas a la construcción del proyecto. Por ejemplo, hay plugins para compilar, copiar recursos, ejecutar pruebas, generar JAR o producir documentación.

Piensa en Maven como el **coordinador** de una secuencia de trabajo. El coordinador decide qué tareas corresponden a cada momento y los plugins proporcionan la funcionalidad necesaria para realizarlas.

| Plugin | Tarea que realiza | Cuándo lo encontramos |
|---|---|---|
| `maven-compiler-plugin` | Compilar Java | Al ejecutar `mvn compile` |
| `maven-resources-plugin` | Copiar recursos | Durante la preparación de recursos |
| `maven-surefire-plugin` | Ejecutar pruebas unitarias | Al ejecutar `mvn test` |
| `maven-jar-plugin` | Crear el JAR | En `mvn package`, para proyectos JAR |
| `maven-clean-plugin` | Eliminar resultados anteriores | Al ejecutar `mvn clean` |
| `maven-dependency-plugin` | Examinar dependencias | Con órdenes como `mvn dependency:tree` |
| `exec-maven-plugin` | Ejecutar programas desde Maven | Al invocar `mvn exec:java` si está configurado |

!!! info "No confundas plugins y dependencias"
    Una **dependencia** suele ser una biblioteca que necesita nuestro código, como Gson. Un **plugin** es una herramienta que utiliza Maven durante la construcción. Ambos se declaran mediante coordenadas, pero cumplen funciones diferentes.

## 11.2. Fase, plugin y goal: tres conceptos distintos

Estos términos aparecen constantemente en la documentación de Maven:

- **Fase (*phase*):** momento del ciclo de vida, como `compile`, `test` o `package`.
- **Plugin:** componente que aporta una o varias tareas.
- **Objetivo (*goal*):** tarea concreta que ofrece un plugin; por ejemplo, `compiler:compile`.

Por ejemplo, al ejecutar:

```bash
mvn compile
```

solicitamos la **fase** `compile`. En un proyecto Java convencional de tipo `jar`, Maven llega a esa fase ejecutando las tareas vinculadas a ella, entre las que se encuentra el objetivo de compilación del **Maven Compiler Plugin**.

En cambio, con:

```bash
mvn compiler:compile
```

invocamos directamente un **objetivo** de un plugin. Esta orden **no equivale necesariamente** a recorrer todas las fases anteriores del ciclo de vida, por lo que para el trabajo habitual preferimos `mvn compile`.

| Orden | Qué solicitamos | Uso habitual |
|---|---|---|
| `mvn compile` | Una fase del ciclo de vida | Compilar el proyecto correctamente |
| `mvn test` | Una fase del ciclo de vida | Compilar y ejecutar pruebas |
| `mvn package` | Una fase del ciclo de vida | Obtener el artefacto |
| `mvn dependency:tree` | Un goal de plugin | Inspeccionar las dependencias |
| `mvn help:effective-pom` | Un goal de plugin | Consultar la configuración efectiva |

!!! tip "Qué conviene recordar"
    En clase utilizaremos normalmente las **fases** para construir proyectos y algunos **goals** para obtener información o realizar tareas concretas.

## 11.3. ¿Dónde se configuran los plugins?

Los plugins que queremos declarar o configurar en nuestro proyecto se sitúan en el archivo `pom.xml`, dentro de `<build><plugins>`.

Observa el siguiente ejemplo, que fija la versión del plugin de compilación y solicita Java 21:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.14.0</version>
            <configuration>
                <release>21</release>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**Vamos a interpretar cada elemento:**

1. `<build>` agrupa opciones de construcción del proyecto.
2. `<plugins>` contiene los plugins declarados.
3. `<plugin>` representa un plugin concreto.
4. `groupId` y `artifactId` lo identifican.
5. `version` fija la versión del plugin, para evitar depender de una elección implícita.
6. `<configuration>` contiene las opciones que admite ese plugin.
7. `<release>21</release>` configura el compilador para generar código dirigido a Java 21.

**¿Debemos escribir siempre todo esto?** No. Para proyectos sencillos del curso basta normalmente con establecer en `<properties>`:

```xml
<properties>
    <maven.compiler.release>21</maven.compiler.release>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

La propiedad `maven.compiler.release` es reconocida por versiones actuales del plugin de compilación. Configurar explícitamente el plugin tiene sentido cuando queremos fijar su versión o ajustar opciones adicionales.

## 11.4. ¿Por qué se ejecutan plugins que no aparecen en mi POM?

Este comportamiento suele sorprender. Creamos un proyecto Maven muy sencillo, sin escribir ningún plugin, y `mvn package` compila las clases, prepara recursos y crea un JAR.

Sucede porque Maven define **vinculaciones predeterminadas entre fases y objetivos** según el tipo de empaquetado del proyecto. En un proyecto `jar`, determinadas tareas ya están asociadas a fases habituales.

Por tanto, **que un plugin no aparezca escrito en nuestro POM no significa que Maven no lo utilice**.

Podemos investigar la configuración que interpreta Maven mediante:

```bash
mvn help:effective-pom -Doutput=pom-efectivo.xml
```

Abre `pom-efectivo.xml` y busca `maven-compiler-plugin`. El archivo permite consultar la configuración resultante. **No debes editar el POM efectivo:** modifica el `pom.xml` original.

## 11.5. Ejemplo práctico: ejecutar nuestra clase desde Maven

Imagina un proyecto con esta clase en `src/main/java/es/dam/App.java`:

```java
package es.dam;

public class App {
    public static void main(String[] args) {
        System.out.println("Hola desde Maven");
    }
}
```

Si queremos ejecutar esta clase desde Maven, podemos utilizar `exec-maven-plugin`.

**Paso 1.** Añade dentro del `pom.xml`, como hijo directo de `<project>`, el bloque:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>3.5.0</version>
            <configuration>
                <mainClass>es.dam.App</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**Paso 2.** Guarda el archivo y abre la terminal en la carpeta que contiene `pom.xml`.

**Paso 3.** Compila el proyecto:

```bash
mvn compile
```

**Paso 4.** Ejecuta la clase:

```bash
mvn exec:java
```

Deberías ver `Hola desde Maven` entre los mensajes de ejecución.

**¿Qué hemos conseguido?** Maven ha invocado un goal de un plugin que permite ejecutar una clase Java. No hemos generado un programa instalable ni un JAR que incluya todas las dependencias. Son tareas diferentes.

!!! warning "Si ya tienes una sección `<build>`"
    No crees dos secciones `<build>` en el mismo POM. Añade el nuevo `<plugin>` dentro del bloque `<plugins>` existente.

## 11.6. Ejemplo de plugin de diagnóstico: árbol de dependencias

No todos los plugins modifican archivos. Algunos sirven para **investigar el proyecto**. Por ejemplo:

```bash
mvn dependency:tree
```

Esta orden muestra las dependencias directas y transitivas. Es útil cuando una biblioteca incorpora otras bibliotecas y queremos comprender qué está resolviendo Maven.

Este es un buen ejemplo de un *goal* que invocamos directamente sin pedir una fase como `compile` o `package`.

## 11.7. ¿Qué relación tienen los plugins con las pruebas y los JAR?

Durante `mvn test`, Maven puede ejecutar pruebas Java mediante `maven-surefire-plugin`. El framework de pruebas, por ejemplo JUnit, **es una dependencia externa**, mientras que Surefire es el **plugin que las ejecuta**. No necesitamos estudiar JUnit en profundidad para comprender esta relación.

De forma parecida, en un proyecto con empaquetado `jar`, Maven utiliza `maven-jar-plugin` para crear el archivo de salida durante `mvn package`. En el capítulo de empaquetado veremos qué contiene ese JAR y qué ocurre con sus dependencias.

## 11.8. Errores frecuentes

| Problema | Causa habitual | Qué revisar |
|---|---|---|
| `Plugin ... could not be resolved` | Descarga fallida, repositorio inaccesible o coordenadas incorrectas | Conexión, proxy, versión y repositorios |
| `No plugin found for prefix` | Maven no localiza el plugin invocado por su prefijo | Nombre, coordenadas y configuración |
| `ClassNotFoundException` al ejecutar `exec:java` | Clase principal mal escrita o proyecto no compilado | Paquete, `mainClass` y `mvn compile` |
| `release version 21 not supported` | Compilador utilizado no compatible con Java 21 | JDK y `mvn -version`; toolchains si existen |
| XML inválido al añadir un plugin | Etiquetas mal cerradas o segundo bloque `<build>` | Estructura del `pom.xml` |

## 11.9. Actividad guiada

En un proyecto Java Maven sencillo:

1. Comprueba que `mvn -version` muestra un entorno Java adecuado.
2. Ejecuta `mvn clean compile`.
3. Añade y configura `exec-maven-plugin` para ejecutar tu clase principal.
4. Ejecuta `mvn exec:java`.
5. Ejecuta `mvn dependency:tree`.
6. Explica por escrito qué órdenes ejecutan **fases** y cuáles ejecutan **goals**.

??? success "Solución orientativa"
    - `mvn clean compile` invoca fases de los ciclos `clean` y `default`.
    - `mvn exec:java` invoca el objetivo `java` del plugin `exec-maven-plugin`.
    - `mvn dependency:tree` invoca el objetivo `tree` del plugin `maven-dependency-plugin`.
    - El plugin de ejecución no sustituye al empaquetado: permite ejecutar una clase desde Maven.

## 11.10. Comprueba que lo has entendido

**1. ¿Maven compila Java por sí solo o utiliza un plugin?**

??? success "Mostrar solución"
    Maven coordina la construcción. La tarea de compilación la proporciona el `maven-compiler-plugin`, que utiliza las herramientas de compilación Java correspondientes.

**2. ¿`mvn package` es un goal de plugin?**

??? success "Mostrar solución"
    No. `package` es una fase del ciclo de vida. Para completarla Maven ejecuta las tareas asociadas a las fases necesarias, utilizando plugins.

**3. ¿Una dependencia Gson y el plugin Surefire cumplen la misma función?**

??? success "Mostrar solución"
    No. Gson es una biblioteca que puede utilizar nuestro programa. Surefire es un plugin que Maven emplea para ejecutar pruebas.

**4. ¿Por qué puede funcionar `mvn compile` sin declarar el plugin en el POM?**

??? success "Mostrar solución"
    Porque Maven dispone de vinculaciones predeterminadas entre fases y objetivos de plugins para tipos de proyecto habituales, como `jar`.

**5. ¿Qué archivo modificamos si queremos cambiar la configuración de un plugin?**

??? success "Mostrar solución"
    El `pom.xml` del proyecto (o su POM padre si hereda esa configuración), no el POM efectivo generado.

## Recursos oficiales

- [Introducción a los plugins de Maven](https://maven.apache.org/guides/introduction/introduction-to-plugins.html)
- [Referencia del ciclo de vida](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
- [Maven Compiler Plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)
- [Maven Surefire Plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)
