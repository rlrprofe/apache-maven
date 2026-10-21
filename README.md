# Apache Maven · Minicurso práctico

Curso MkDocs Material centrado en Maven, con **21 capítulos secuenciales**, cuatro bloques temáticos y ejemplos Java 21. Incluye los proyectos de descarga y el flujo de publicación de GitHub Actions.

## Instalar y previsualizar

```bash
python -m pip install -r requirements.txt
mkdocs serve
```

## Publicación

Sube **todo el contenido de esta carpeta** a la raíz del repositorio. El `mkdocs.yml` organiza la navegación por bloques. El índice de la derecha de cada página se genera automáticamente a partir de los encabezados `##` y `###`.

**Importante:** elimina del repositorio los Markdown antiguos con nombres como `08-pom-avanzado.md`, `13-recursos.md`, `14-pruebas.md`, `20-avanzado.md` y los archivos con numeración antigua. Para evitar páginas huérfanas, lo más seguro es sustituir completamente la carpeta `docs/` por la incluida en este ZIP.
