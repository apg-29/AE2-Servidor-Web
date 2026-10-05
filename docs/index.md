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

## Despliegue con nginx

Una vez generado el site estático con Zensical, se copia su contenido a un directorio destinado a los sitios web de nginx:

```bash
sudo mkdir -p /var/www/ae2
sudo cp -r site/* /var/www/ae2/
```

Luego crearemos un servidor en nginx:

```bash
vim /etc/nginx/sites-available/ae2-zensical
```

Con la siguiente configuración:

```text
server {
    listen 80;
    listen [::]:80;

    server_name ae2.com;

    root /var/www/ae2;
    index index.html;
}
```

El site se activa a través de un enlace simbólico a `sites-available`:

```bash
sudo ln -s /etc/nginx/sites-available/ae2-zensical /etc/nginx/sites-enabled/
```

Antes de iniciar nginx, debemos comprobar que la configuración no devuelve errores:

```bash
sudo nginx -t
```

Y finalmente se recarga o inicia el servicio:

```bash
sudo nginx -s reload
sudo systemctl start nginx
```

#### Configuración del dominio

Para que `ae2.com` sea accesible habrá que añadir una entrada en `/etc/hosts`:

```text
10.0.2.15 imw.2asir.com miapp.com ae2.com
```
