# 10. Comandos esenciales

!!! info "Dónde encaja este capítulo"
    En esta unidad reunimos las órdenes que utilizarás en clase. Antes de memorizar un comando, pregúntate **qué resultado necesitas**: compilar, ejecutar pruebas, empaquetar o investigar un error.


**Al finalizar podrás:** seleccionar el comando apropiado según el problema.

## Comandos esenciales

| Comando | Para qué sirve | Cuándo usarlo |
|---|---|---|
| `mvn -version` | Diagnóstico de Maven/JDK | Al instalar o cambiar Java |
| `mvn validate` | Verifica estructura/POM | Al iniciar un proyecto |
| `mvn compile` | Compila aplicación | Tras modificar código principal |
| `mvn test` | Ejecuta pruebas unitarias | Antes de entregar |
| `mvn package` | Genera artefacto | Para distribuir |
| `mvn clean package` | Limpia y construye | Para evitar restos |
| `mvn verify` | Ejecuta verificaciones | Antes de publicar |
| `mvn install` | Instala en repositorio local | Para consumir desde otro proyecto |
| `mvn dependency:tree` | Árbol de dependencias | Ante conflictos |
| `mvn help:effective-pom` | POM resultante | Ante configuraciones heredadas |
| `mvn test -Dtest=CalculadoraTest` | Prueba concreta | Durante depuración |
| `mvn -q test` | Reduce mensajes | Automatización |
| `mvn -e test` | Muestra detalles de error | Diagnóstico |
| `mvn -X test` | Registro detallado | Diagnóstico profundo |

## Diferencia entre fase y objetivo

`mvn package` ejecuta una **fase**. `mvn dependency:tree` invoca un **objetivo de plugin** (goal). La sintaxis general de objetivo es `prefijo:goal`; también se puede indicar el plugin y su versión explícitamente.

## Ejecutar varios objetivos

```bash
mvn clean test
mvn clean package
```

Los argumentos se procesan en el orden solicitado. La construcción se ejecuta desde la carpeta que contiene el POM, salvo que indiques otro con `-f`.

```bash
mvn -f ruta/al/pom.xml test
```

## Diagnóstico guiado

Si `mvn package` falla, identifica primero la fase del error. ¿Falló la descarga de una dependencia, la compilación, una prueba o el empaquetado? **No corrijas un error de prueba eliminando las pruebas.**

## Ejercicio

Quieres comprobar únicamente `CalculadoraTest`. ¿Qué orden usarías?

??? success "Mostrar solución"
    `mvn test -Dtest=CalculadoraTest`. Si no existe una prueba con ese nombre, Surefire puede informar de que no encontró coincidencias.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
