# 16. Maven Wrapper

!!! info "¿Por qué aparece Maven Wrapper en este curso?"
    Dos equipos pueden tener versiones distintas de Maven. El **Wrapper** permite que un proyecto utilice una versión de Maven definida por su configuración, facilitando que alumnado y profesorado construyan el mismo proyecto sin depender exclusivamente de la instalación global.


**Al finalizar podrás:** explicar cómo fijar la versión de Maven utilizada por un proyecto.

## El problema de las versiones

Un compañero puede tener Maven diferente del tuyo. **Maven Wrapper** incorpora scripts y configuración al repositorio para que se descargue y ejecute una versión concreta de Maven.

Archivos habituales:

```text
mvnw
mvnw.cmd
.mvn/wrapper/maven-wrapper.properties
```

## Uso

=== "Windows"
    ```powershell
    .\mvnw.cmd -version
    .\mvnw.cmd clean test
    ```

=== "Linux"
    ```bash
    ./mvnw -version
    ./mvnw clean test
    ```

El Wrapper **no reemplaza al JDK**. Java sigue siendo necesario. En algunos entornos debes dar permisos de ejecución a `mvnw` (`chmod +x mvnw`).

## Generar Wrapper

Desde un proyecto Maven, con un plugin Wrapper disponible:

```bash
mvn wrapper:wrapper
```

Revisa la versión generada y los archivos incorporados antes de publicarlos. El Wrapper puede necesitar acceso a Internet la primera vez.

## Seguridad

Revisa los scripts y URLs del Wrapper cuando procedan de repositorios desconocidos. Evita descargar y ejecutar scripts de procedencia dudosa sin inspeccionarlos.

## Ejercicio

Si el proyecto incluye `mvnw`, ¿es obligatorio instalar Maven globalmente?

??? success "Mostrar solución"
    Normalmente no: el Wrapper puede obtener la versión de Maven configurada. Sí necesitas un JDK compatible y, para la primera descarga, acceso a los recursos necesarios.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
