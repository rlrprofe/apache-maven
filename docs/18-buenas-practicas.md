# 18. Buenas prácticas

!!! info "Objetivo del capítulo"
    No basta con que `mvn package` funcione una vez en nuestro ordenador. Un proyecto bien preparado debe ser **comprensible, reproducible y fácil de compartir**. Estas recomendaciones se centran en Maven y su estructura, no en convenciones generales de programación Java.


**Al finalizar podrás:** organizar proyectos compartibles, seguros y mantenibles.

## Recomendaciones

1. Mantén un `README.md` que explique requisitos, compilación, ejecución y pruebas.
2. Declara explícitamente la versión de Java (`maven.compiler.release`).
3. Fija versiones de dependencias y plugins importantes; revísalas periódicamente.
4. Evita dependencias innecesarias y consulta `mvn dependency:tree`.
5. Usa `scope=test` para bibliotecas de pruebas.
6. No incluyas `target/`, contraseñas ni tokens en Git.
7. Evita rutas absolutas del equipo del profesor.
8. Usa paquetes descriptivos, nombres claros y Javadoc donde aporte contexto.
9. Ejecuta `mvn clean verify` antes de publicar cuando el proyecto lo permita.
10. Documenta cómo generar el JAR y si incluye dependencias.
11. No desactives pruebas para ocultar fallos.
12. Mantén separados código, recursos y pruebas.

## Ejemplo de `.gitignore`

```gitignore
target/
.idea/
*.log
.env
```

Los archivos `.vscode/` pueden contener configuraciones compartibles o locales: decide caso por caso qué versionar.

## Checklist de entrega

- [ ] El proyecto compila desde terminal sin depender del IDE.
- [ ] El POM usa Java 21.
- [ ] Las pruebas pasan y están documentadas.
- [ ] El README identifica la clase principal.
- [ ] No se publican credenciales.
- [ ] Se incluyen los recursos necesarios.
- [ ] Se puede reconstruir el artefacto tras borrar `target/`.

## Ejercicio

¿Deberías subir el directorio `~/.m2/repository` al repositorio del proyecto?

??? success "Mostrar solución"
    No. Es una caché local de artefactos. Debes compartir el POM y el código; Maven resolverá las dependencias en el equipo de destino.

---

!!! tip "Cómo estudiar este apartado"
    Ejecuta los comandos en un proyecto de pruebas, anota el resultado y despliega la solución solo después de intentarlo.
