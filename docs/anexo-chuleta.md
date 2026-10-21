# Anexo. Chuleta de comandos

**Al finalizar podrás:** consultar rápidamente las órdenes más útiles.

```bash
# Entorno
mvn -version

# Construcción
mvn validate
mvn compile
mvn test
mvn package
mvn clean package
mvn verify
mvn install

# Diagnóstico
mvn dependency:tree
mvn dependency:analyze
mvn help:effective-pom
mvn help:active-profiles
mvn -e test
mvn -X test

# Opciones
mvn -o test                   # modo sin conexión
mvn -U test                   # comprueba actualizaciones
mvn -q test                   # salida reducida
mvn test -Dtest=CalculadoraTest
mvn -f otra-carpeta/pom.xml test

# Plugins
mvn exec:java
mvn javadoc:javadoc

# Multimódulo
mvn -pl aplicacion -am test
```

!!! warning "Diferencias entre sistemas"
    Los comandos Maven son esencialmente los mismos en Windows y Linux. Lo que cambia es la sintaxis de rutas, variables de entorno y scripts (`mvnw.cmd` frente a `./mvnw`).

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
