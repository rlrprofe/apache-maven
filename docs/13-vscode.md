# 13. Maven en VS Code

**Objetivos:** crear un proyecto con un arquetipo, corregir la configuración antigua de Java, importar un proyecto existente, ejecutar fases Maven y diferenciar el JDK del IDE del utilizado por Maven.

## 13.1. Preparar VS Code

Necesitamos:

1. **JDK 21** instalado.
2. **Apache Maven** disponible en la terminal (compruébalo con `mvn -version`).
3. **Visual Studio Code**.
4. Extensiones **Extension Pack for Java** y **Maven for Java**.

Abre **Extensiones** con `Ctrl + Shift + X`, busca cada extensión por su nombre e instálala. Si VS Code solicita reiniciarse, hazlo.

!!! note "¿Qué aporta cada extensión?"
    - **Extension Pack for Java**: soporte para Java, ejecución, depuración y pruebas.
    - **Maven for Java**: vista de proyectos Maven, creación e invocación de objetivos.

## 13.2. Crear un proyecto Maven desde cero

Vamos a crear `hola-vscode`.

**Paso 1.** Pulsa `Ctrl + Shift + P` para abrir la paleta de comandos.

**Paso 2.** Escribe y selecciona:

```text
Maven: Create Maven Project
```

**Paso 3.** Selecciona el arquetipo:

```text
maven-archetype-quickstart
```

**Paso 4.** Si la versión más reciente que ofrece tu catálogo es **1.4**, selecciona **1.4**. Es válida como plantilla, aunque habrá que actualizar la configuración de Java.

**Paso 5.** Introduce los datos solicitados:

| Campo | Ejemplo | Qué representa |
|---|---|---|
| `groupId` | `es.dam` | Grupo/organización |
| `artifactId` | `hola-vscode` | Nombre técnico del proyecto |
| `version` | `1.0-SNAPSHOT` | Versión del proyecto |
| `package` | `es.dam` | Paquete Java de las clases |

**Paso 6.** Elige una carpeta del equipo en la que crear el proyecto. Espera a que Maven termine y abre la carpeta resultante en VS Code.

!!! tip "Si aparece una terminal de confirmación"
    Según la extensión y el arquetipo, puede solicitar confirmación de los datos. Revisa los valores y acepta la generación cuando se te indique.

## 13.3. Examinar la estructura generada

En el explorador verás algo parecido a:

```text
hola-vscode/
├── pom.xml
├── src/
│   ├── main/java/es/dam/App.java
│   └── test/java/es/dam/AppTest.java
└── target/                 ← aparecerá después de compilar
```

- `pom.xml`: configuración Maven.
- `src/main/java`: código de la aplicación.
- `src/test/java`: pruebas automatizadas.
- `target`: archivos generados por Maven; normalmente no se suben al repositorio.

## 13.4. Corregir Java 7 en Quickstart 1.4

Abre `pom.xml`. El arquetipo puede generar:

```xml
<maven.compiler.source>1.7</maven.compiler.source>
<maven.compiler.target>1.7</maven.compiler.target>
```

Estas líneas solicitan Java 7. **No indican que tengas instalado JDK 7.** Son valores antiguos de la plantilla.

**Sustitúyelas** por:

```xml
<maven.compiler.release>21</maven.compiler.release>
```

El bloque completo de propiedades puede quedar así:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.release>21</maven.compiler.release>
</properties>
```

Guarda con `Ctrl + S`.

!!! warning "Si aparecen errores de plugins o pruebas"
    Quickstart 1.4 puede incluir dependencias o plugins antiguos. El cambio anterior corrige el nivel de Java, pero quizá debas actualizar otros componentes si se producen errores adicionales. No elimines pruebas ni dependencias al azar: revisa primero el mensaje concreto de Maven.

## 13.5. Compilar y probar desde la terminal

Abre **Terminal → Nuevo terminal**. Comprueba que estás en la carpeta que contiene `pom.xml`.

```bash
mvn -version
mvn clean compile
mvn test
mvn package
```

- `mvn -version`: versión de Maven y Java que lo ejecuta.
- `mvn clean compile`: limpia y compila.
- `mvn test`: ejecuta pruebas si están configuradas.
- `mvn package`: crea el artefacto del proyecto.

Si `compile` funciona pero `test` falla, **no significa necesariamente que Java esté mal instalado**. Puede haber una prueba incorrecta o una versión antigua de un plugin de pruebas.

## 13.6. Ejecutar una clase Java

Abre `App.java` y localiza `public static void main(String[] args)`.

Puedes ejecutarla de dos maneras:

**Opción A.** Pulsa **Run** sobre el método `main` o utiliza el comando de ejecución de Java de VS Code.

**Opción B.** Ejecuta Maven mediante un plugin apropiado, por ejemplo:

```bash
mvn exec:java -Dexec.mainClass=es.dam.App
```

En este segundo caso, Maven necesita resolver el plugin de ejecución. El nombre de clase debe coincidir con el paquete real del archivo `App.java`.

!!! tip "¿Qué opción conviene en clase?"
    Utiliza **Run Java** para probar rápidamente el código y la **terminal Maven** para comprobar que el proyecto se construye de manera reproducible fuera del IDE.

## 13.7. Importar un proyecto Maven existente

1. Descarga y descomprime `01-hola-maven` desde el curso.
2. En VS Code, selecciona **Archivo → Abrir carpeta**.
3. Elige la carpeta que contiene `pom.xml`, no solo `src`.
4. Espera a que las extensiones Java reconozcan el proyecto.
5. Abre `pom.xml` y comprueba `maven.compiler.release=21`.
6. Ejecuta `mvn clean package`.
7. Localiza el resultado en `target`.

No necesitas volver a crear un proyecto cuando ya existe un `pom.xml` válido.

## 13.8. ¿Cómo seleccionar el JDK correcto?

En VS Code intervienen varias configuraciones que no debemos confundir:

| Configuración | Para qué sirve | Dónde comprobar |
|---|---|---|
| Java que ejecuta Maven en la terminal | Inicia Maven | `mvn -version` |
| JDK asignado al proyecto Java en VS Code | Compilar/analizar/ejecutar desde el IDE | `Java: Configure Java Runtime` |
| Java objetivo del proyecto | Nivel de Java deseado | `pom.xml` → `maven.compiler.release` |

**Paso a paso:**

1. Pulsa `Ctrl + Shift + P`.
2. Ejecuta `Java: Configure Java Runtime`.
3. Comprueba que JDK 21 está detectado y disponible para el proyecto.
4. Abre la terminal integrada y ejecuta `mvn -version`.
5. Revisa `pom.xml`.

!!! info "Importante"
    El servidor de lenguaje de Java puede utilizar una JVM distinta de la utilizada para ejecutar tu proyecto. Si VS Code compila con un JDK y Maven desde terminal utiliza otro, pueden aparecer resultados diferentes.

## 13.9. Ejecutar Maven desde el panel del IDE

La extensión **Maven for Java** ofrece una vista de proyectos Maven. Allí puedes expandir el proyecto y localizar objetivos del ciclo de vida como `clean`, `compile`, `test` y `package`.

También puedes ejecutarlos desde la terminal. Ambas formas utilizan Maven, aunque su configuración concreta puede depender del entorno y de la extensión.

## 13.10. Solución de problemas frecuentes

| Problema | Causa probable | Acción |
|---|---|---|
| `mvn` no se reconoce | Maven no está en `Path` | Revisar instalación y reiniciar terminal |
| `Source option 7 is no longer supported` | Quickstart 1.4 configurado para Java 7 | Sustituir `source`/`target` por `release=21` |
| `release version 21 not supported` | Compilador demasiado antiguo | Comprobar JDK y `mvn -version` |
| No aparece el proyecto Maven | Carpeta equivocada o importación pendiente | Abrir carpeta con `pom.xml` y esperar |
| Importación Java en rojo | Configuración no actualizada | Guardar POM y ejecutar `Java: Clean Java Language Server Workspace` si hace falta |
| Fallan las pruebas | Código de prueba, dependencia o plugin | Leer el error de `mvn test` y revisar el POM |

## 13.11. Actividad guiada

1. Crea `hola-vscode` con Quickstart 1.4.
2. Localiza el `pom.xml` y anota la versión de Java original.
3. Cambia la configuración a Java 21.
4. Modifica el mensaje de `App.java`.
5. Ejecuta `mvn clean compile` y después `mvn package`.
6. Abre el proyecto `01-hola-maven` del curso y compáralo con el que has creado.

### Preguntas con soluciones

**1. ¿Qué diferencia hay entre crear e importar un proyecto?**

??? success "Mostrar solución"
    Crear utiliza una plantilla para generar archivos nuevos. Importar abre un proyecto existente que ya contiene su propio `pom.xml`.

**2. ¿Por qué Quickstart 1.4 puede fallar con JDK 21?**

??? success "Mostrar solución"
    Porque la plantilla configura el proyecto para Java 7 mediante `source` y `target` con valor `1.7`, no admitidos por el compilador de JDK 21.

**3. ¿Basta con seleccionar JDK 21 en VS Code para cambiar el POM?**

??? success "Mostrar solución"
    No. Debemos revisar la configuración de Java del proyecto en `pom.xml`, además del JDK seleccionado en el IDE.

**4. ¿Por qué conviene ejecutar `mvn package` desde terminal aunque el botón Run funcione?**

??? success "Mostrar solución"
    Porque permite comprobar que la construcción funciona mediante Maven y no depende exclusivamente de la configuración de ejecución del IDE.

## Documentación oficial

- [Maven for Java en VS Code](https://code.visualstudio.com/docs/java/java-build)
- [Gestión de Java en VS Code](https://code.visualstudio.com/docs/java/java-project)
