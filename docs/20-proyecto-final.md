# 20. Proyecto integrador

!!! info "Qué se evalúa aquí"
    Aunque utilizaremos una aplicación Java sencilla como ejemplo, **el objetivo principal es demostrar que sabes gestionar un proyecto con Maven**: estructurarlo, configurar el POM, resolver dependencias, compilar, ejecutar pruebas y empaquetar. No se pretende evaluar el diseño avanzado de la aplicación.


**Al finalizar podrás:** integrar Maven, dependencias, pruebas, recursos y distribución.

## Enunciado

Desarrolla una **agenda de contactos de consola** que permita registrar contactos, listarlos, buscarlos por nombre y exportarlos a JSON. Utiliza Maven, Java 21 y Gson. El objetivo es practicar la estructura de un proyecto profesional pequeño.

## Requisitos funcionales

- Cada contacto tendrá nombre, correo y teléfono.
- No se admiten nombres vacíos ni correos sin formato básico válido.
- Se pueden añadir, listar y buscar contactos.
- Se exportará la colección a un archivo JSON en una ruta elegida por el usuario.
- El programa mostrará mensajes comprensibles ante entradas inválidas.

## Requisitos técnicos

- `pom.xml` con versión de Java 21.
- Gson como dependencia de compilación.
- JUnit 5 con `scope=test`.
- Al menos cinco pruebas automatizadas de validación y búsqueda.
- Modelo con atributos privados, constructor, getters y setters.
- Separación entre `app`, `modelo`, `servicio` y `persistencia`.
- README con instalación, compilación, ejecución, pruebas y empaquetado.
- JAR ejecutable con dependencias mediante Shade.

## Plan de trabajo

1. Diseña las clases y responsabilidades antes de escribir el menú.
2. Implementa el modelo y las validaciones; añade pruebas.
3. Implementa operaciones de agenda y compruébalas sin interfaz.
4. Añade el menú y el exportador JSON.
5. Ejecuta `mvn clean verify` y construye el JAR.
6. Prueba el JAR desde una carpeta distinta.

## Entrega

Sube el proyecto Maven sin `target/`, incluye capturas de ejecución y pruebas, y explica en una página cómo Maven resuelve las dependencias y genera el artefacto.

## Rúbrica orientativa (10 puntos)

| Aspecto | Puntos |
|---|---:|
| Funcionalidad y validación | 3 |
| Arquitectura y claridad | 2 |
| POM, dependencias y empaquetado | 2 |
| Pruebas automatizadas | 2 |
| README y evidencias | 1 |

## Orientaciones desplegables

??? success "Diseño de clases propuesto"
    `Contacto`: datos y accesores; `AgendaService`: altas, validación y búsqueda; `JsonExporter`: serialización y escritura; `MenuAgenda`: entrada y salida. El servicio no debería depender de `Scanner`.

??? success "Pruebas mínimas recomendadas"
    Prueba alta correcta, nombre vacío, correo inválido, búsqueda existente y búsqueda inexistente. Añade al menos una prueba de exportación a JSON usando una carpeta temporal de JUnit.

??? success "Cómo comprobar el empaquetado"
    Ejecuta `mvn clean verify`, después `java -jar target/NOMBRE-DEL-JAR.jar` usando el nombre real producido. Si faltan dependencias, revisa la configuración del plugin Shade.

!!! note "Solución completa"
    No se incluye una implementación íntegra para evitar sustituir el trabajo de consolidación. Sí se ofrecen ejemplos previos completos y orientaciones comprobables.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
