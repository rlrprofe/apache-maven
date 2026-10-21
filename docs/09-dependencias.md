# 9. Dependencias y repositorios

!!! info "Dónde encaja este capítulo"
    Hasta ahora hemos compilado proyectos sencillos. En un proyecto real solemos necesitar bibliotecas externas, como un controlador JDBC o una biblioteca JSON. Maven permite **declarar esas bibliotecas en el POM y resolverlas** sin copiar manualmente todos los JAR al proyecto.


**Al finalizar podrás:** añadir bibliotecas, entender ámbitos y diagnosticar transitivas.

## Añadir una dependencia

En el proyecto de JSON descargable encontrarás:

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.11.0</version>
</dependency>
```

Maven busca el artefacto en repositorios configurados (normalmente Maven Central), lo descarga y lo guarda en la caché local, habitualmente `~/.m2/repository`.

## Dependencias transitivas

Si A depende de B y B depende de C, Maven puede incorporar C sin que aparezca explícitamente en tu POM. Esto facilita el uso de bibliotecas, pero requiere revisar versiones y conflictos.

```bash
mvn dependency:tree
mvn dependency:analyze
```

`dependency:tree` ayuda a descubrir qué dependencia introdujo otra y en qué versión se resolvió.

## Ámbitos (*scopes*)

| Scope | Idea principal |
|---|---|
| `compile` | Predeterminado; disponible para compilar y ejecutar |
| `provided` | Necesario para compilar, lo aporta el entorno de ejecución |
| `runtime` | Necesario al ejecutar, no para compilar código principal |
| `test` | Solo para compilar y ejecutar pruebas |
| `system` | Ligado a ruta local; desaconsejado |
| `import` | Usado para importar POM de gestión en `dependencyManagement` |

## Conflictos y versiones

Dos bibliotecas pueden solicitar versiones distintas de una tercera. Maven aplica reglas de mediación de dependencias (por ejemplo, cercanía en el árbol). No presupongas que siempre se escoge la más reciente.

## Trabajo sin conexión

```bash
mvn -o test
```

El modo `-o` (*offline*) solo funciona si los plugins y dependencias requeridos ya están en la caché.

## Ejercicio

¿Por qué JUnit suele declararse con `<scope>test</scope>`?

??? success "Mostrar solución"
    Porque el código de pruebas lo necesita, pero no debería formar parte de las dependencias de ejecución de la aplicación productiva.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
