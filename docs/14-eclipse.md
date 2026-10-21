# 14. Maven en Eclipse

**Objetivos:** crear e importar proyectos Maven, comprobar JDK 21, sincronizar cambios del POM, ejecutar pruebas y generar un JAR.

## 14.1. Eclipse y Maven: ¿qué es m2e?

Eclipse dispone de integración con Maven mediante **m2e** (*Maven Integration for Eclipse*), incluida habitualmente en Eclipse IDE for Java Developers.

Esta integración permite reconocer `pom.xml`, descargar dependencias, configurar el proyecto Java y ejecutar objetivos Maven desde los menús del IDE.

!!! note "Maven integrado y Maven externo"
    Eclipse puede utilizar una distribución de Maven integrada distinta de la que ejecutas en la terminal del sistema. También puede usar un JDK diferente. Si los resultados no coinciden, revisa ambas configuraciones.

## 14.2. Comprobar JDK 21 en Eclipse

1. Abre **Window → Preferences** (en algunos sistemas, las preferencias se encuentran en otro menú).
2. Ve a **Java → Installed JREs**.
3. Comprueba que JDK 21 está disponible; si no aparece, añádelo indicando su carpeta de instalación.
4. Revisa **Java → Compiler** para conocer la configuración general del compilador.
5. En un proyecto Maven, recuerda que **el `pom.xml` también configura la versión de Java**.

!!! warning "No confundir"
    Que Eclipse pueda ejecutar Java 21 no significa que todos los proyectos Maven estén configurados automáticamente para Java 21. Debemos revisar `maven.compiler.release` en cada POM.

## 14.3. Importar un proyecto Maven existente

Utilizaremos el proyecto descargable `01-hola-maven`.

**Paso 1.** Descarga el ZIP y descomprímelo.

**Paso 2.** Abre Eclipse y selecciona:

```text
File → Import → Maven → Existing Maven Projects
```

**Paso 3.** En **Root Directory**, selecciona la carpeta que contiene `pom.xml`.

**Paso 4.** Comprueba que Eclipse detecta el proyecto y pulsa **Finish**.

**Paso 5.** Espera a que termine la importación y, si es necesario, la descarga de dependencias.

**Paso 6.** Expande el proyecto en **Package Explorer** y localiza:

```text
src/main/java
src/test/java
pom.xml
```

**Paso 7.** Abre `pom.xml` y verifica:

```xml
<maven.compiler.release>21</maven.compiler.release>
```

## 14.4. Crear un proyecto Maven nuevo en Eclipse

1. Selecciona **File → New → Other...**.
2. Busca **Maven → Maven Project**.
3. Sigue el asistente y elige un arquetipo Java básico, como `maven-archetype-quickstart`, si aparece disponible.
4. Introduce, por ejemplo:

| Campo | Valor |
|---|---|
| `groupId` | `es.dam` |
| `artifactId` | `hola-eclipse` |
| `version` | `1.0-SNAPSHOT` |
| `package` | `es.dam` |

5. Finaliza el asistente y espera a que Eclipse genere el proyecto.
6. **Revisa siempre `pom.xml`**: si el arquetipo ha configurado Java 7 mediante `source=1.7` y `target=1.7`, sustitúyelos por `maven.compiler.release=21`.

!!! tip "La plantilla puede ser antigua"
    Igual que ocurre en VS Code, Eclipse puede ofrecer arquetipos con configuraciones de Java o dependencias antiguas. La versión del arquetipo no indica la versión del JDK.

## 14.5. Ejecutar una aplicación Java

Abre la clase que contiene:

```java
public static void main(String[] args)
```

Haz clic derecho sobre la clase y selecciona:

```text
Run As → Java Application
```

Comprueba la salida en la consola de Eclipse.

**¿Esto es lo mismo que ejecutar Maven?** No exactamente. Aquí ejecutamos una clase Java desde el IDE. Para comprobar la construcción del proyecto con Maven utilizaremos objetivos del ciclo de vida.

## 14.6. Ejecutar Maven desde Eclipse

Haz clic derecho sobre el proyecto y busca:

```text
Run As → Maven build...
```

En el campo **Goals** escribe:

```text
clean package
```

Ejecuta la configuración y observa la consola. Si la construcción termina correctamente, localiza el JAR en `target`.

También puedes utilizar `test` para ejecutar pruebas, o `clean compile` para compilar desde cero.

## 14.7. Actualizar dependencias y configuración

Supongamos que añadimos Gson al POM:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.11.0</version>
    </dependency>
</dependencies>
```

Después de guardar, Eclipse puede sincronizar automáticamente el proyecto, pero si no lo hace:

1. Haz clic derecho sobre el proyecto.
2. Selecciona **Maven → Update Project...**.
3. Marca el proyecto correspondiente.
4. Confirma la actualización.
5. Revisa el apartado **Maven Dependencies** y la vista **Problems**.

**¿Qué hace Update Project?** Sincroniza la configuración de Eclipse con el POM y las dependencias resueltas. No equivale a ejecutar automáticamente `mvn clean package`.

## 14.8. ¿Qué versión de Maven utiliza Eclipse?

Abre:

```text
Window → Preferences → Maven → Installations
```

Podrás consultar las instalaciones disponibles. Si quieres utilizar la misma distribución que en terminal, añade o selecciona la instalación externa correspondiente.

También revisa las opciones de **Maven → User Settings** cuando necesites configurar repositorios, proxies o el archivo `settings.xml`.

!!! info "Comprobación adicional"
    El comando `mvn -version` ejecutado en una terminal externa informa de Maven y Java de esa terminal. No demuestra por sí solo qué distribución y JDK está usando una ejecución Maven lanzada desde Eclipse.

## 14.9. Errores frecuentes

| Situación | Posible explicación | Qué hacer |
|---|---|---|
| No aparece `Maven Project` | Integración m2e ausente o no disponible | Revisar instalación de Eclipse |
| Eclipse no detecta el proyecto | Seleccionaste `src` en lugar de la raíz | Elegir carpeta con `pom.xml` |
| Dependencias sin resolver | Descarga fallida o proyecto sin sincronizar | Guardar POM y ejecutar `Maven → Update Project` |
| Error con Java 7 | Arquetipo antiguo | Configurar `release=21` |
| Compila en Eclipse pero falla en terminal | Diferente Maven o JDK | Comparar instalaciones y configuración |
| No aparece el JAR | No se ha ejecutado `package` o hubo error | Revisar consola y carpeta `target` |

## 14.10. Práctica guiada

1. Importa `01-hola-maven` en Eclipse.
2. Comprueba que el POM solicita Java 21.
3. Ejecuta `App` como aplicación Java.
4. Modifica el mensaje que aparece por consola.
5. Ejecuta **Run As → Maven build...** con `clean package`.
6. Comprueba el archivo generado en `target`.
7. Añade una dependencia de ejemplo, guarda el POM y ejecuta **Maven → Update Project**.

### Preguntas con soluciones

**1. ¿Qué diferencia hay entre `Run As → Java Application` y `Run As → Maven build`?**

??? success "Mostrar solución"
    La primera opción ejecuta una clase Java desde el IDE. La segunda invoca objetivos Maven, como compilar, ejecutar pruebas y empaquetar el proyecto.

**2. ¿Qué hace `Maven → Update Project`?**

??? success "Mostrar solución"
    Sincroniza el proyecto Eclipse con la configuración de Maven y sus dependencias. No sustituye a una construcción completa.

**3. ¿Por qué podría funcionar el proyecto en Eclipse y fallar en la terminal?**

??? success "Mostrar solución"
    Porque Eclipse y la terminal pueden utilizar diferentes instalaciones de Maven, diferentes JDK o distintas configuraciones del entorno.

**4. ¿Dónde revisarías la distribución Maven utilizada por Eclipse?**

??? success "Mostrar solución"
    En `Window → Preferences → Maven → Installations`.

## Documentación oficial

- [Maven Integration for Eclipse](https://eclipse.dev/m2e/)
- [Introducción a Maven](https://maven.apache.org/guides/getting-started/)
