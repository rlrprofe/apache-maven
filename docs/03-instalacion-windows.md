# 3. Instalación en Windows

**Objetivos de aprendizaje**

Al finalizar este apartado podrás identificar los requisitos de Java de distintas versiones de Maven, instalar ambas herramientas en Windows, configurar las variables de entorno, comprobar la instalación y resolver los problemas más frecuentes.

## 3.1. ¿Qué versión de Java necesita Maven?

Apache Maven está desarrollado en Java y necesita una instalación compatible de Java para ejecutarse. **No exige necesariamente JDK 21**: el requisito depende de la versión de Maven.

| Versión de Maven | Java mínimo para ejecutar Maven | Observaciones |
|---|---|---|
| Maven 3.8.x | Java 7 | Rama antigua; no recomendada para nuevas instalaciones. |
| Maven 3.9.x | Java 8 | Rama Maven 3, adecuada para este curso. |
| Maven 4.x | Java 17 | Comprueba el estado y los requisitos de la versión concreta antes de instalarla. |

!!! note "Versión utilizada en este curso"
    Para las prácticas trabajaremos con **JDK 21** y una versión estable de **Apache Maven 3** (por ejemplo, Maven 3.9.x). Java 21 es nuestra elección para desarrollar los proyectos, **no el mínimo exigido por Maven 3**.

Consulta siempre los requisitos de la versión descargada en la [documentación oficial](https://maven.apache.org/install.html) y las [descargas oficiales](https://maven.apache.org/download.cgi).

## 3.2. Ejecutar Maven no es lo mismo que compilar Java

Al ejecutar `mvn -version`, Maven informa de **su propia versión** y de la **versión de Java con la que se está ejecutando**.

```powershell
mvn -version
```

Ejemplo orientativo de salida:

```text
Apache Maven 3.9.9
Maven home: C:\apache-maven-3.9.9
Java version: 21.0.x
Java home: C:\jdk-21
OS name: "windows 11"
```

En cambio, cuando ejecutas `mvn compile`, Maven coordina la compilación mediante un plugin y un compilador Java. Si el `pom.xml` contiene:

```xml
<properties>
    <maven.compiler.release>21</maven.compiler.release>
</properties>
```

el proyecto se compila para **Java 21**, por lo que se necesita acceso a un compilador compatible (habitualmente JDK 21 o posterior). Maven puede configurarse para utilizar otro JDK de compilación mediante **Maven Toolchains**.

!!! example "Dos versiones que no debes confundir"
    - **Java que ejecuta Maven:** aparece en `mvn -version`.
    - **Java objetivo del proyecto:** se indica, por ejemplo, mediante `maven.compiler.release` en `pom.xml`.

## 3.3. Paso 1. Instalar y comprobar el JDK

Para este curso instala un **JDK 21** desde una distribución de OpenJDK, por ejemplo [Eclipse Temurin](https://adoptium.net/).

Abre PowerShell y ejecuta:

```powershell
java -version
javac -version
```

- `java -version` indica la versión de Java invocada desde la terminal.
- `javac -version` indica la versión del compilador Java.

Si `javac` no está disponible, revisa si has instalado un **JDK** y no solo un entorno de ejecución.

En los siguientes ejemplos supondremos que el JDK está en `C:\jdk-21`. **Sustituye esta ruta por la real de tu equipo.**

## 3.4. Paso 2. Descargar y descomprimir Maven

1. Abre <https://maven.apache.org/download.cgi>.
2. Descarga el **Binary zip archive** de una versión estable de Maven 3; por ejemplo, `apache-maven-3.9.9-bin.zip`.
3. Descomprime el archivo, por ejemplo, en `C:\apache-maven-3.9.9`.
4. Comprueba que dentro de la carpeta existe `bin\mvn.cmd`.

```text
C:\apache-maven-3.9.9\
├── bin\
├── boot\
├── conf\
└── lib\
```

!!! tip "¿Por qué descargamos el ZIP binario?"
    Contiene Maven preparado para ejecutarse. El archivo *source* contiene el código fuente de Maven y no es necesario para instalarlo.

## 3.5. Paso 3. Configurar las variables de entorno

En Windows, busca **Editar las variables de entorno del sistema** y abre **Variables de entorno**.

Configura estas variables con las rutas reales:

```text
JAVA_HOME=C:\jdk-21
MAVEN_HOME=C:\apache-maven-3.9.9
```

En la variable `Path`, añade:

```text
%JAVA_HOME%\bin
%MAVEN_HOME%\bin
```

`JAVA_HOME` debe apuntar a la **raíz del JDK**, no a su carpeta `bin`. `MAVEN_HOME` es útil para identificar la instalación, aunque no es imprescindible si añades directamente la carpeta `bin` de Maven al `Path`.

Cierra PowerShell y abre una terminal nueva para que se apliquen los cambios.

## 3.6. Paso 4. Verificar la instalación

```powershell
java -version
javac -version
mvn -version
```

Comprueba que aparecen las versiones esperadas. Para inspeccionar las rutas utilizadas:

```powershell
$env:JAVA_HOME
$env:MAVEN_HOME
Get-Command mvn
```

| Dato de `mvn -version` | Significado |
|---|---|
| `Apache Maven` | Versión instalada de Maven. |
| `Maven home` | Carpeta desde la que se ejecuta Maven. |
| `Java version` | Versión de Java con la que se ejecuta Maven. |
| `Java home` | Ubicación de ese Java. |

## 3.7. Paso 5. Probar Maven con un proyecto

Sitúate en la carpeta de un proyecto que contenga `pom.xml` y ejecuta:

```powershell
mvn validate
mvn compile
mvn test
mvn package
```

- `validate`: comprueba la configuración básica del proyecto.
- `compile`: compila el código principal.
- `test`: ejecuta las pruebas configuradas.
- `package`: genera el artefacto, normalmente un archivo JAR.

!!! info "La primera ejecución puede tardar"
    Maven puede necesitar descargar plugins y dependencias. Esto es normal cuando todavía no están en el repositorio local.

## 3.8. Errores frecuentes

| Problema | Posible causa | Qué comprobar |
|---|---|---|
| `mvn` no se reconoce | Maven no está en `Path`. | Que `%MAVEN_HOME%\bin` exista y esté en `Path`. |
| `JAVA_HOME` no está definido correctamente | Ruta inexistente o apunta a `bin`. | Que apunte a la raíz del JDK. |
| `javac` no se reconoce | JDK ausente o `Path` incorrecto. | Instalación y ruta `%JAVA_HOME%\bin`. |
| Maven utiliza otro Java | Hay varias instalaciones de Java. | `mvn -version` y `JAVA_HOME`. |
| `release version 21 not supported` | El compilador no admite Java 21. | JDK utilizado para compilar y configuración de Toolchains. |
| Error descargando dependencias | Red, proxy o repositorio. | Conexión y configuración de Maven. |
| No se encuentra `pom.xml` | Terminal en carpeta incorrecta. | Ejecutar desde la raíz del proyecto. |

## 3.9. Actividades de autoevaluación

**1. ¿Puede Maven 3.9 ejecutarse con Java 17?**

??? success "Mostrar solución · Actividad 1"
    Sí. Maven 3.9 requiere como mínimo Java 8, por lo que Java 17 es compatible.

**2. ¿Por qué utilizamos JDK 21 en este curso?**

??? success "Mostrar solución · Actividad 2"
    Porque es la versión elegida para desarrollar y compilar los proyectos de las prácticas, no porque sea el mínimo exigido por Maven 3.

**3. ¿Qué información proporciona `mvn -version`?**

??? success "Mostrar solución · Actividad 3"
    Muestra la **versión de Apache Maven** y datos de su entorno, incluidos la versión y la ubicación de Java con la que Maven se ejecuta.

**4. Si un proyecto contiene `<maven.compiler.release>21</maven.compiler.release>`, ¿significa que Maven necesita Java 21 para ejecutarse?**

??? success "Mostrar solución · Actividad 4"
    No. Esa propiedad indica la versión objetivo del proyecto. Maven puede ejecutarse con otra versión compatible de Java, pero para compilar necesitará un compilador que admita Java 21.

**5. ¿Por qué debes abrir otra terminal tras modificar las variables de entorno?**

??? success "Mostrar solución · Actividad 5"
    Porque una terminal abierta antes del cambio puede seguir utilizando las variables anteriores.

## 3.10. Recursos oficiales

- [Descarga de Apache Maven](https://maven.apache.org/download.cgi)
- [Instalación oficial](https://maven.apache.org/install.html)
- [Historial de versiones](https://maven.apache.org/docs/history.html)
- [Documentación de Maven](https://maven.apache.org/guides/)
- [Eclipse Temurin / OpenJDK](https://adoptium.net/)
