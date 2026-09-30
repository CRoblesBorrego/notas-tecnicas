---
type: note
aliases:
  - wp-load.php
  - Insertar contenido en WordPress por script
  - Adjuntos sin upload_max_filesize
tags:
  - desarrollo
  - wordpress
  - docker
  - automatizacion
---

# Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI

La imagen oficial de WordPress **no trae WP-CLI**. Para cargar contenido de forma programada en un contenedor así, la alternativa habitual es la REST API, que obliga a crear contraseñas de aplicación, a registrar cada campo meta y a pelearse con los límites de subida. Hay una vía más corta: un fichero PHP que empiece por

```php
require_once '/var/www/html/wp-load.php';
```

y se ejecute con el binario `php` que ya está en la imagen. A partir de ahí se dispone de **toda la API interna** —`wp_insert_post()`, `wp_set_post_categories()`, `stick_post()`, los enganches, los filtros— sin autenticación HTTP de por medio.

Cuatro detalles que deciden si funciona:

- **Ejecutar como el usuario del servidor web**, no como root. Si no, los ficheros creados quedan con propietario equivocado y el servidor no puede escribir sobre ellos después.
- **Los límites de subida no aplican.** `upload_max_filesize` y `post_max_size` (con frecuencia 2 MB por defecto) solo gobiernan las peticiones HTTP. Copiando el fichero a `wp_upload_dir()['path']` y llamando después a `wp_insert_attachment()` se puede adjuntar material mucho mayor sin tocar la configuración de PHP.
- **Las miniaturas no se generan solas.** Hace falta `require_once ABSPATH . 'wp-admin/includes/image.php'` y luego `wp_update_attachment_metadata($id, wp_generate_attachment_metadata($id, $ruta))`. Sin eso el adjunto existe pero solo en tamaño original, y cualquier plantilla que pida un tamaño intermedio recibe la imagen completa.
- **El texto alternativo no es un campo del adjunto**, sino el meta `_wp_attachment_image_alt`. El pie de foto sí lo es: viaja en `post_excerpt`.

Para que el script sea **reejecutable**, la entrada y cada adjunto se buscan por slug antes de crearse. Con una advertencia: `wp_insert_attachment()` puede reescribir el slug que se le pasa, así que conviene fijarlo con un `wp_update_post()` posterior — y aun así la búsqueda tiene su propia trampa, descrita en [Los adjuntos de una entrada en borrador son invisibles para WP_Query sin usuario](Los%20adjuntos%20de%20una%20entrada%20en%20borrador%20son%20invisibles%20para%20WP_Query%20sin%20usuario.md).

El compromiso frente a la REST API: se gana potencia y se pierde el saneado y el control de permisos que la capa REST aplica por defecto. Un script así **escribe lo que le digan**, incluido HTML que la API habría filtrado. Vale para carga controlada desde una máquina de confianza; no vale como puerta abierta a un sistema externo.

## Fuentes

- [`wp_insert_attachment()` — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/wp_insert_attachment/)
- [`wp_generate_attachment_metadata()` — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/wp_generate_attachment_metadata/)

## Relacionado

- [Los adjuntos de una entrada en borrador son invisibles para WP_Query sin usuario](Los%20adjuntos%20de%20una%20entrada%20en%20borrador%20son%20invisibles%20para%20WP_Query%20sin%20usuario.md)
- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
