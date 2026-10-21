# Apache Maven · Minicurso práctico

Este minicurso enseña a **crear, configurar, compilar y mantener proyectos Java con Apache Maven**, tanto desde la terminal como desde **Visual Studio Code, Eclipse e IntelliJ IDEA**. No pretende enseñar Java, JUnit ni desarrollo web desde cero: cuando aparecen, es únicamente para mostrar qué hace Maven con ellos.

Trabajaremos con **JDK 21** como referencia para los ejemplos. La versión de Java que necesita Maven para ejecutarse depende de la versión de Maven instalada: **JDK 21 es una elección del curso, no un requisito universal de Maven**.

!!! info "Cómo está organizado el sitio"
    El **menú lateral izquierdo** está dividido en cuatro bloques de aprendizaje. En esta página, el **índice de la derecha** también muestra esos cuatro bloques para que puedas acceder a ellos directamente. Dentro de cada capítulo, el índice derecho recoge sus apartados y subapartados.

## Bloque 1 · Fundamentos e instalación

**Objetivo:** comprender qué es Maven, preparar Java y Maven y reconocer la estructura de un proyecto.

1. [Qué es Apache Maven](01-introduccion.md): problema que resuelve y vocabulario básico.
2. [Requisitos y preparación](02-requisitos.md): JDK, variables de entorno y comprobaciones.
3. [Instalación en Windows](03-instalacion-windows.md): descarga, `JAVA_HOME`, `Path` y verificación.
4. [Instalación en Linux](04-instalacion-linux.md): paquetes e instalación manual.
5. [Primer proyecto Maven](05-primer-proyecto.md): generar y compilar un proyecto sencillo.
6. [Estructura de proyectos](06-estructura.md): carpetas `src` y `target`, código y recursos.

## Bloque 2 · Funcionamiento de Maven

**Objetivo:** aprender a interpretar el `pom.xml` y utilizar Maven para resolver dependencias, ejecutar fases y empaquetar proyectos.

7. [El archivo `pom.xml`](07-pom-basico.md): GAV, propiedades, Java 21 y el problema de Java 7 en Quickstart 1.4.
8. [Ciclo de vida](08-ciclo-vida.md): fases y orden de ejecución.
9. [Dependencias y repositorios](09-dependencias.md): cómo se descargan bibliotecas.
10. [Comandos esenciales](10-comandos.md): órdenes más utilizadas y su propósito.
11. [Plugins de Maven](11-plugins.md): diferencia entre fases, plugins y objetivos.
12. [Empaquetado con Maven](12-empaquetado.md): `package`, JAR y plugins de empaquetado.

## Bloque 3 · Maven desde distintos IDE

**Objetivo:** crear, importar, actualizar y compilar el mismo tipo de proyecto Maven desde distintas herramientas.

13. [Maven en VS Code](13-vscode.md): Quickstart 1.4, corrección del POM y tareas Maven.
14. [Maven en Eclipse](14-eclipse.md): integración m2e y actualización del proyecto.
15. [Maven en IntelliJ IDEA](15-intellij.md): importación, sincronización y JDK.

## Bloque 4 · Prácticas y resolución de problemas

**Objetivo:** trabajar de manera autónoma, diagnosticar errores y completar un proyecto gestionado con Maven.

16. [Maven Wrapper](16-wrapper.md): usar una versión de Maven definida para el proyecto.
17. [Errores y diagnóstico](17-diagnostico.md): problemas habituales y cómo investigarlos.
18. [Buenas prácticas](18-buenas-practicas.md): organización y entrega.
19. [Prácticas guiadas](19-practicas.md): actividades progresivas.
20. [Proyecto integrador](20-proyecto-final.md): aplicar Maven de principio a fin.
21. [Autoevaluación](21-evaluacion.md): comprobar lo aprendido.

## Material de consulta

- [Glosario](anexo-glosario.md): términos fundamentales.
- [Chuleta de comandos](anexo-chuleta.md): referencia rápida.
- [Descargas](descargas.md): proyectos de ejemplo.

!!! tip "Orden recomendado"
    Sigue los bloques 1 y 2 en orden. Después puedes trabajar con el IDE que utilices en clase y consultar los otros dos como comparación. Termina con las prácticas y el proyecto integrador.
