# AE2 — Servidor web

## Introducción

Esta documentación recoge el proceso realizado para desplegar un sitio web estático utilizando Zensical y NGINX.

Se configurarán dos servidores virtuales mediante NGINX y se comprobará el acceso a los sitios desplegados.

- Preparar un proyecto de documentación con Zensical.
- Generar un sitio web estático.
- Instalar y configurar NGINX.
- Configurar dos servidores virtuales.
- Desplegar sitios estáticos.
- Comprobar el funcionamiento de la configuración

## Creación del site

Generamos la estructura inicial del site, incluyendo el archivo `zensical.toml` y los documentos iniciales:

```bash
uv run zensical new .
```

Comprobaremos que funciona y que el servidor permite visualizar el site:

```bash
uv run zensical serve
```
