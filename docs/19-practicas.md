# 19. Prácticas guiadas

!!! info "Cómo realizar estas prácticas"
    Las actividades están pensadas para que aprendas Maven mediante **observación, modificación del POM y comprobación de resultados**. No es necesario desarrollar aplicaciones complejas: el código Java es solo el material con el que verificaremos la construcción.


**Al finalizar podrás:** realizar actividades de instalación, compilación, dependencias, pruebas e IDE.

Cada práctica incluye un resultado observable y una propuesta de solución. **No consultes la solución antes de ejecutar los pasos**. Descarga los ZIP desde [Descargas](descargas.md).

## Práctica 1 · Diagnóstico del entorno

1. Ejecuta `java -version`, `javac -version` y `mvn -version`.
2. Anota versiones y rutas. Comprueba que Maven usa Java 21.
3. Explica qué ocurriría si `JAVA_HOME` apuntara a `bin`.
4. Entrega una captura de terminal y un párrafo de interpretación.

??? success "Solución y comprobaciones"
    Deben aparecer tres comandos válidos y `mvn -version` debe indicar Java 21. `JAVA_HOME` apunta a la raíz del JDK. Si apunta a `bin`, Maven puede fallar al localizar correctamente la instalación.

## Práctica 2 · Primera compilación

1. Descomprime `01-hola-maven.zip`.
2. Abre terminal en la carpeta que contiene `pom.xml`.
3. Ejecuta `mvn validate`, `mvn compile` y `mvn package`.
4. Localiza los `.class` y el `.jar`.
5. Ejecuta `mvn exec:java` y cambia el mensaje mostrado.

??? success "Solución y comprobaciones"
    Las clases compiladas aparecen en `target/classes` y el JAR en `target/`. `mvn exec:java` imprime el mensaje de `App`. Al modificar la clase, recompila antes de ejecutar si es necesario.

## Práctica 3 · Modificar el POM

1. Cambia `artifactId` por `hola-personalizado`.
2. Cambia la versión a `1.1-SNAPSHOT`.
3. Ejecuta `mvn clean package`.
4. Explica el nuevo nombre del artefacto.

??? success "Solución y comprobaciones"
    Con configuración estándar, el artefacto será `target/hola-personalizado-1.1-SNAPSHOT.jar`. Los identificadores del POM se utilizan para nombrar y localizar artefactos.

## Práctica 4 · Dependencia Gson

1. Abre `03-json-gson.zip`.
2. Localiza Gson en `pom.xml`.
3. Ejecuta `mvn dependency:tree` y `mvn exec:java`.
4. Modifica los datos del objeto serializado.
5. Explica por qué la aplicación necesita Gson en tiempo de ejecución.

??? success "Solución y comprobaciones"
    El árbol muestra Gson como dependencia. La aplicación genera un JSON con los datos cambiados. Gson participa en la serialización y debe estar disponible durante la ejecución.

## Práctica 5 · Pruebas JUnit

1. Abre `02-calculadora-junit.zip`.
2. Ejecuta `mvn test` y localiza `target/surefire-reports`.
3. Cambia temporalmente una aserción para provocar un fallo.
4. Revisa el informe y restaura la prueba.
5. Ejecuta `mvn clean verify`.

??? success "Solución y comprobaciones"
    La primera ejecución termina con éxito; al cambiar la aserción aparece un fallo de prueba; al restaurarla vuelve a finalizar correctamente. La fase `verify` incluye las fases anteriores pertinentes.

## Práctica 6 · JAR ejecutable

1. En `01-hola-maven`, ejecuta `mvn clean package`.
2. Ejecuta `java -jar target/hola-maven-1.0-SNAPSHOT.jar`.
3. Abre `03-json-gson` y construye su JAR con Shade.
4. Ejecuta el JAR con dependencias y explica la diferencia.

??? success "Solución y comprobaciones"
    El primer JAR contiene una aplicación sin dependencias externas. El proyecto Gson necesita incorporar la biblioteca en el JAR sombreado o proporcionarla por classpath; el ejemplo incluye Shade.

## Práctica 7 · VS Code

1. Abre `02-calculadora-junit` desde **Archivo → Abrir carpeta**.
2. Espera a la importación Java.
3. Ejecuta la prueba desde el IDE y desde terminal.
4. Comprueba el JDK configurado.

??? success "Solución y comprobaciones"
    Ambas ejecuciones deben pasar. Si el IDE y terminal discrepan, compara su JDK y la configuración del proyecto.

## Práctica 8 · Eclipse

1. Importa `01-hola-maven` como **Existing Maven Projects**.
2. Ejecuta `App` y `Run As → Maven build...` con `clean package`.
3. Cambia la versión del POM y usa **Maven → Update Project**.

??? success "Solución y comprobaciones"
    Eclipse reconoce el POM, ejecuta la clase y genera el artefacto. Tras sincronizar, debe reflejar los cambios de configuración.

## Práctica 9 · IntelliJ IDEA

1. Abre `03-json-gson`.
2. Revisa Project SDK (Java 21).
3. Ejecuta el objetivo `package` desde el panel Maven.
4. Cambia una dependencia y recarga los proyectos Maven.

??? success "Solución y comprobaciones"
    El proyecto se importa desde el POM y el JAR se construye en `target`. La recarga sincroniza el modelo de dependencias.

## Práctica 10 · Investigación de errores

Provoca **uno cada vez**: un `artifactId` inexistente de una dependencia, un `release` de Java no soportado por tu JDK de pruebas o una prueba que falla. Para cada caso, registra síntoma, causa, solución y comando de verificación. No alteres tu JDK principal para forzar errores.

??? success "Orientación de solución"
    Dependencia inexistente → error de resolución; `release` incompatible → error del compilador; aserción incorrecta → fallo de Surefire. Restaura cada cambio y ejecuta `mvn clean test`.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
