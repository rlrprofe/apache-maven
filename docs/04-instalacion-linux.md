# 4. Instalación en Linux

**Al finalizar podrás:** instalar Maven mediante gestor de paquetes o archivo oficial.

## Opción A. Gestor de paquetes (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install openjdk-21-jdk maven
java -version
javac -version
mvn -version
```

Es la opción más cómoda. **La versión de Maven depende de los repositorios de tu distribución**, y puede no coincidir con la última publicada.

## Opción B. Instalación manual

1. Descarga el archivo binario `.tar.gz` desde [Maven](https://maven.apache.org/download.cgi).
2. Verifica su integridad siguiendo las instrucciones oficiales.
3. Extrae el archivo y ubícalo en una carpeta como `/opt/apache-maven-VERSION`.
4. Configura `PATH` y, si lo deseas, `MAVEN_HOME`.

```bash
# Ejemplo: sustituye VERSION por la carpeta real
export MAVEN_HOME=/opt/apache-maven-VERSION
export PATH="$MAVEN_HOME/bin:$PATH"
mvn -version
```

Las líneas anteriores solo afectan a la sesión actual. Para persistirlas, incorpóralas a la configuración apropiada de tu shell (por ejemplo `~/.bashrc` si usas Bash interactivo), y abre una nueva terminal.

## Localiza el JDK

```bash
readlink -f "$(command -v javac)"
java -version
javac -version
```

La ruta de `JAVA_HOME` debe apuntar a la carpeta del JDK, **por encima de `bin`**. En Ubuntu/Debian suele tener una forma similar a `/usr/lib/jvm/java-21-openjdk-amd64`, pero debes verificar la ruta de tu equipo.

## Conflictos entre versiones

Si `mvn -version` utiliza un Java diferente al esperado, revisa `JAVA_HOME`, `PATH` y la selección de alternativas de Java. No asumas que `java -version` y Maven siempre usan el mismo ejecutable.

## Práctica

Ejecuta `mvn -version` y anota Maven home, Java version y Java home.

??? success "Qué debe aparecer"
    Debe verse una versión de Apache Maven, una versión de Java compatible y una ruta de instalación de Java. No importa que las rutas exactas sean distintas a las de tus compañeros.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
