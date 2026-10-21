# 2. Requisitos y preparación

**Al finalizar podrás:** comprobar la versión de Java y entender `JAVA_HOME` y `PATH`.

## Qué necesitas

- **JDK 21**, no solo un JRE.
- **Apache Maven 3.9.x o versión compatible** descargada desde la web oficial.
- Terminal: PowerShell en Windows; Bash en Linux.
- IDE opcional. La terminal será nuestra referencia común.

Consulta [Apache Maven](https://maven.apache.org/download.cgi), [instalación oficial](https://maven.apache.org/install.html) y [OpenJDK](https://jdk.java.net/).

## ¿Qué son las variables de entorno?

`JAVA_HOME` apunta a la **carpeta raíz del JDK**, no a `bin` ni a `java.exe`. `PATH` enumera carpetas donde el sistema busca ejecutables. Si añades `%JAVA_HOME%\bin` en Windows o `$JAVA_HOME/bin` en Linux, podrás escribir `java` desde cualquier carpeta.

```text
JAVA_HOME = C:\jdk-21
PATH      = ...;C:\jdk-21\bin;...
```

La variable `MAVEN_HOME` es opcional para la ejecución básica; lo imprescindible es que `mvn` sea accesible desde `PATH`.

## Diagnóstico inicial

=== "Windows (PowerShell)"
    ```powershell
    java -version
    javac -version
    mvn -version
    Get-Command java
    Get-Command mvn
    ```

=== "Linux (Bash)"
    ```bash
    java -version
    javac -version
    mvn -version
    command -v java
    command -v mvn
    ```

`java -version` informa del entorno de ejecución; `javac -version`, del compilador; `mvn -version`, de Maven y de la JVM que utiliza Maven.

**Pregunta.** `java -version` funciona, pero `javac -version` falla. ¿Qué sospechas?

??? success "Mostrar solución"
    Puede estar instalado solo un entorno de ejecución, o el `PATH` puede apuntar a una instalación distinta del JDK. Hay que verificar `JAVA_HOME` y las rutas de los ejecutables.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
