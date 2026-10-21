# 6. Estructura de proyectos

**Al finalizar podrás:** localizar código, pruebas, recursos y archivos generados.

## Convención antes que configuración

Maven espera encontrar los archivos en ubicaciones predecibles:

```text
mi-proyecto/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/          ← código de la aplicación
│   │   └── resources/     ← configuración y recursos
│   └── test/
│       ├── java/          ← pruebas automatizadas
│       └── resources/     ← datos exclusivos de pruebas
└── target/                ← generado por Maven
```

La carpeta `target/` se crea al construir el proyecto; **no debes editarla manualmente ni subirla a Git**. `mvn clean` la elimina para empezar de nuevo.

## Paquetes Java y carpetas

Si una clase declara `package es.dam.maven;`, su ubicación convencional es `src/main/java/es/dam/maven/App.java`. El nombre del paquete no tiene por qué coincidir con el `groupId`, aunque normalmente se alinean.

## Recursos: qué hace Maven con los archivos que no son Java

No todos los archivos de un proyecto son clases Java. A veces necesitamos textos, ficheros `.properties`, plantillas o ejemplos JSON. Maven reserva para ellos la carpeta `src/main/resources`. **Lo que nos interesa en este curso no es programar la lectura de esos archivos, sino entender cómo Maven los incorpora a la construcción.**

Cuando ejecutamos `mvn process-resources` o avanzamos hasta `mvn package`, el **Maven Resources Plugin** copia normalmente los recursos de `src/main/resources` a `target/classes`, conservando sus rutas relativas. Los recursos de `src/test/resources` se preparan por separado para las pruebas.

**Práctica de observación:** crea `src/main/resources/mensaje.txt` con el texto `Hola desde Maven`. Ejecuta:

```bash
mvn process-resources
```

Abre `target/classes/mensaje.txt` y comprueba que Maven ha copiado el archivo. Después ejecuta `mvn clean`: verás que desaparece `target/`, pero **el original de `src/main/resources` permanece intacto**.

!!! warning "Recursos y datos sensibles"
    No guardes contraseñas reales, tokens ni credenciales en `src/main/resources` si el archivo va a formar parte del proyecto o del JAR. Maven puede copiar esos archivos al artefacto final. En este minicurso no profundizaremos en la gestión de secretos.


Un archivo `src/main/resources/mensaje.txt` se copia a `target/classes/mensaje.txt`. Se puede leer desde el classpath con `getResourceAsStream("/mensaje.txt")` o mediante el cargador de clases adecuado. Evita rutas absolutas que solo funcionen en tu ordenador.

## Ejercicio

¿Dónde colocarías una prueba de `Calculadora` y un archivo de datos que solo necesita esa prueba?

??? success "Mostrar solución"
    `src/test/java/.../CalculadoraTest.java` para la prueba y `src/test/resources/` para sus datos. El código productivo permanece en `src/main/java`.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
