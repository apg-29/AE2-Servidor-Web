---
layout: post
title: "Jekyll y NGINX"
date: 2026-10-06
categories: jekyll update
---

Jekyll se encarga de generar los archivos estáticos de la página web.

Una vez generado el sitio, NGINX puede servir esos archivos directamente a los usuarios.

En esta práctica se ha configurado un segundo servidor virtual de NGINX para separar la web creada con Jekyll del sitio de documentación creado con Zensical.

El resultado final permite tener varios sitios web funcionando en el mismo servidor.
