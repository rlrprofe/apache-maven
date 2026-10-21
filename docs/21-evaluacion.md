# 21. Autoevaluación

!!! info "Qué debes comprobar"
    La autoevaluación se centra en las competencias de Maven: instalación, POM, ciclo de vida, dependencias, plugins, empaquetado, integración con IDE y diagnóstico de errores.


**Al finalizar podrás:** verificar comprensión del ciclo Maven sin depender del IDE.

Responde sin consultar los apuntes. Despliega las soluciones al finalizar.

**1.** ¿Qué tres elementos forman las coordenadas GAV?

??? success "Respuesta 1"
    `groupId`, `artifactId` y `version`.

**2.** ¿Qué diferencia existe entre `compile` y `test`?

??? success "Respuesta 2"
    `compile` compila código principal; `test` alcanza además la compilación y ejecución de las pruebas correspondientes.

**3.** ¿Dónde se guardan las dependencias descargadas habitualmente?

??? success "Respuesta 3"
    En el repositorio Maven local, normalmente `~/.m2/repository`.

**4.** ¿Para qué sirve `dependency:tree`?

??? success "Respuesta 4"
    Para visualizar dependencias directas y transitivas y sus versiones resueltas.

**5.** ¿Por qué `mvn package` puede fallar si una prueba falla?

??? success "Respuesta 5"
    Porque `package` ejecuta las fases previas pertinentes, incluida `test`.

**6.** ¿Qué es el POM efectivo?

??? success "Respuesta 6"
    La configuración resultante de combinar el POM con herencia, perfiles activos y valores predeterminados.

**7.** ¿Qué diferencia existe entre `install` y `deploy`?

??? success "Respuesta 7"
    `install` coloca el artefacto en el repositorio Maven local; `deploy` publica en un repositorio Maven remoto configurado.

**8.** ¿Por qué no debes subir `target/` a Git?

??? success "Respuesta 8"
    Contiene resultados generados que Maven puede reconstruir a partir de fuentes y POM.

**9.** ¿Qué es el Wrapper?

??? success "Respuesta 9"
    Un conjunto de scripts/configuración para utilizar una versión definida de Maven sin exigir una instalación global.

**10.** ¿Cómo distingues un JAR ejecutable de uno con dependencias incluidas?

??? success "Respuesta 10"
    El primero indica la clase principal; el segundo incorpora además las bibliotecas necesarias, habitualmente mediante Shade u otra estrategia de empaquetado.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
