# 17. Errores y diagnóstico

!!! info "Método de trabajo"
    Un error de Maven no siempre es un error del código Java. Puede proceder del JDK, del POM, de una dependencia, de un plugin o de la conexión a un repositorio. Aprenderemos a identificar **en qué etapa falla la construcción** antes de modificar archivos al azar.


**Al finalizar podrás:** clasificar errores antes de modificar el proyecto.

## Método de diagnóstico

1. Reproduce el error desde la terminal: `mvn clean test`.
2. Identifica **la primera causa relevante**, no solo el último mensaje.
3. Distingue error de **entorno**, **dependencias**, **compilación**, **pruebas** o **ejecución**.
4. Aplica una corrección pequeña y vuelve a ejecutar.
5. Documenta el problema y su solución.

| Error | Interpretación | Comprobación |
|---|---|---|
| `mvn: command not found` | Maven no accesible | `PATH` / instalación |
| `JAVA_HOME is not defined correctly` | Ruta JDK incorrecta | `JAVA_HOME`, `javac` |
| `release version 21 not supported` | Compilador demasiado antiguo | `mvn -version` |
| `Could not resolve dependencies` | Descarga o coordenadas | Red, repositorio, GAV |
| `package ... does not exist` | Dependencia o import | POM y `dependency:tree` |
| `Tests run: ..., Failures: 1` | Prueba fallida | Informe Surefire |
| `no main manifest attribute` | JAR sin `Main-Class` | Configuración `maven-jar-plugin` |
| `NoClassDefFoundError` | Falta clase en ejecución | Classpath y empaquetado |
| `ClassNotFoundException` | Clase no encontrada | Paquete, ruta, compilación |

## Comandos útiles

```bash
mvn -version
mvn -e clean test
mvn dependency:tree
mvn help:effective-pom
```

`-X` proporciona más detalle, pero puede producir una salida extensa. No publiques logs con credenciales o tokens.

## Ejercicio

Compilas con `<maven.compiler.release>21</maven.compiler.release>` y aparece `release version 21 not supported`. ¿Qué revisas primero?

??? success "Mostrar solución"
    `mvn -version`: probablemente Maven está usando un JDK anterior a 21. Cambia `JAVA_HOME` o la configuración correspondiente y vuelve a comprobar.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
