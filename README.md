# AE2 — Servidor Web

Despliegue de un sitio estático utilizando Zensical y NGINX.

## Iniciar uv e instalar Zensical

Iniciamos el proyecto con uv:

```bash
uv init  
```

Añadimos Zensical como dependencia de desarrollo:

```bash
uv add --dev zensical
```  

Comprobamos que Zensical está instalado correctamente:

```bash
uv run zensical --version  
```

## Crear el proyecto de documentación

Creamos la estructura inicial de Zensical:

```bash
uv run zensical new .  
```

Esto genera el directorio `docs/` con los archivos iniciales de documentación y el archivo de configuración `zensical.toml`.

La estructura principal del proyecto:

```text
AE2/  
├── docs/  
├── src/  
├── zensical.toml  
├── pyproject.toml  
├── uv.lock  
└── README.md  
```

## Generar el sitio estático

Para generar el sitio estático ejecutamos:

```bash
uv run zensical build  
```

Zensical genera los archivos HTML, CSS, JavaScript y demás recursos dentro del directorio `site/`.  

El directorio `site/` contiene los archivos generados y no el código de la documentación.

## Probar la documentación localmente

Durante el desarrollo podemos utilizar el servidor de Zensical:

```bash
uv run zensical serve  
```

Este servidor se utiliza únicamente para comprobar la documentación durante el desarrollo.

## Despliegue del site con NGINX

Comprobamos el estado del servicio:

```bash
sudo systemctl status nginx  
```

### Crear el directorio del site

Para desplegar el site utilizamos `/var/www/ae2`:

```bash
sudo mkdir -p /var/www/ae2  
```

Copiamos el contenido generado por Zensical:

```bash
sudo cp -r site/* /var/www/ae2/  
```

De esta forma NGINX sirve los archivos estáticos desde `/var/www/ae2`.

## Configurar el servidor de NGINX

Creamos un nuevo archivo de configuración:

```bash
sudo vim /etc/nginx/sites-available/ae2-zensical  
```

La configuración utilizada es:

```text
server {  
    listen 80;  
    listen \[::\]:80;  
  
    server\_name ae2.com;  
  
    root /var/www/ae2;  
    index index.html;  
}  
```

La directiva `server_name` indica el nombre utilizado para acceder al sitio.

La directiva `root` indica el directorio donde se encuentran los archivos que NGINX debe servir.

## Habilitar el sitio

Creamos un enlace simbólico desde `sites-available` hacia `sites-enabled`:

```bash
sudo ln -s /etc/nginx/sites-available/ae2-zensical /etc/nginx/sites-enabled/  
```

## Comprobar la configuración de NGINX

Antes de iniciar o recargar NGINX comprobamos que la configuración es correcta:

```bash
sudo nginx -t  
```

## Configurar el nombre del servidor

Para poder acceder a `ae2.com` desde la máquina virtual se añadió una entrada en `/etc/hosts`:

```text
10.0.2.15 imw.2asir.com miapp.com ae2.com  
```

De esta forma `ae2.com` apunta a la dirección IP de la máquina virtual.

## Iniciar NGINX

Iniciamos el servicio:

```bash
sudo systemctl start nginx  
```

También podemos recargar la configuración después de realizar cambios:

```bash
sudo systemctl reload nginx  
```

## Comprobar el sitio

Finalmente accedemos desde el navegador a `http://ae2.com`.  

El contenido mostrado corresponde al sitio estático generado con Zensical y servido por NGINX.


Cada vez que se modifica la documentación, es necesario volver a generar el site y volver a copiar los archivos generados al directorio utilizado por NGINX:

```bash
uv run zensical build
sudo cp -r site/\* /var/www/ae2/  
```

Finalmente se puede recargar NGINX:

```bash
sudo nginx -s reload
```

## Despliegue de un sitio con Jekyll

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

El siguiente paso será configurar un segundo servidor virtual de **NGINX** para servir el contenido generado por Jekyll.


