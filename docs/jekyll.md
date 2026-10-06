# Despliegue de un sitio con Jekyll

Como ampliación de la práctica, se ha creado un segundo sitio web utilizando Jekyll.

## Instalación de Ruby

Primero se instaló Ruby utilizando los repositorios del sistema:

```bash
sudo apt install ruby
```

Se comprobó que Ruby y RubyGems estaban correctamente instalados:

```
ruby -v gem -v
```

En este caso se obtuvo:

```text
Ruby 3.2.3 RubyGems 3.4.20
```

## Instalación de Jekyll

A continuación se instalaron Jekyll y Bundler mediante RubyGems:

```bash
sudo gem install jekyll bundler
```

Se comprobó la instalación:

```bash
jekyll -v bundle -v
```

Versiones obtenidas:

```text
Jekyll 4.3.2 Bundler 4.0.22
```

## Creación del proyecto

Se creó un nuevo proyecto Jekyll dentro del proyecto de la práctica:

```bash
jekyll new myblog
```

## Comprobación del sitio

Para comprobar que Jekyll funciona correctamente se ejecutó:

```bash
bundle exec jekyll serve
```

El sitio se pudo visualizar desde el navegador utilizando el servidor de desarrollo de Jekyll.

## Personalización

Se modificó el contenido del sitio para convertirlo en una pequeña página personal.

Se añadió una página Sobre mí mediante about.markdown y se crearon entradas de prueba en _posts/.

Finalmente, se generó el sitio estático mediante:

```bash
bundle exec jekyll build
```

Los archivos generados se encuentran en:

```text
myblog/_site/
```

Esta carpeta contiene los archivos **HTML**, **CSS**, imágenes y demás recursos que serán servidos por **NGINX**.

## Despliegue en NGINX

Se creó un directorio para el segundo sitio:

```bash
sudo mkdir -p /var/www/myblog
```

Después se copiaron los archivos generados por Jekyll:

```bash
sudo cp -r _site/* /var/www/myblog/
```

## Segundo servidor virtual

Se creó un nuevo servidor virtual de **NGINX**:

```bash
sudo nano /etc/nginx/sites-available/myblog
```

La configuración utilizada fue:

```text
server {
    listen 80;
    listen [::]:80;

    server_name myblog.com;

    root /var/www/myblog;
    index index.html;

    location / {
    try_files $uri $uri/ =**404**;
    }
}
```

Se activó mediante un enlace simbólico:

```bash
sudo ln -s /etc/nginx/sites-available/myblog /etc/nginx/sites-enabled/
```

## Comprobación de NGINX

Antes de aplicar la configuración se comprobó que no hubiera errores:

```bash
sudo nginx -t
```

Después se inició **NGINX** y se recargó la configuración:

```bash
sudo systemctl start nginx
sudo nginx -s reload
```

## Configuración del dominio

Para poder acceder al sitio utilizando myblog.com, se añadió el dominio al archivo /etc/hosts:

```text
10.0.2.15 imw.2asir.com miapp.com ae2.com myblog.com
```

## Resultado

Finalmente, se comprobó desde el navegador que el sitio funcionaba correctamente mediante:

[http://myblog.com](http://myblog.com)

De esta forma se han configurado dos sitios independientes utilizando **NGINX**:

```text
**NGINX**
├── ae2.com
│   └── /var/www/ae2
│       └── Sitio generado con Zensical
│
└── myblog.com
    └── /var/www/myblog
    └── Sitio generado con Jekyll
```
