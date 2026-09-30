---
type: note
aliases:
  - Meta por REST en WordPress
  - show_in_rest
  - rest_after_insert_post
tags:
  - desarrollo
  - wordpress
  - integracion
---

# Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas

Cuando un sistema externo —un flujo de automatización, un script, un generador de contenido— crea entradas en WordPress por la REST API y les adjunta campos personalizados, hay dos trampas. Ninguna produce error: la petición devuelve `201 Created` y los datos simplemente no están.

**Primera: el campo tiene que estar registrado con `show_in_rest`.** WordPress **descarta en silencio** cualquier clave del objeto `meta` que no haya sido registrada con `register_post_meta()` (o `register_meta()`) y marcada como visible en REST. No hay aviso, no hay `400`: el resto de la entrada se guarda y el meta desaparece. El registro se hace en `init` y conviene aprovecharlo para dos cosas más que ahí son gratis:

- `sanitize_callback` por campo, para que el saneado ocurra aunque el valor llegue por otra vía (`esc_url_raw` para una URL, `wp_kses_post` para HTML de artículo, `sanitize_text_field` para lo demás).
- `auth_callback`, que decide quién puede escribirlo. Por defecto no es lo que uno espera.

**Segunda: al crear por REST, los meta se escriben *después* de `save_post`.** Un manejador enganchado a `save_post` que intente leer el meta recién enviado leerá vacío, porque en ese momento aún no se ha guardado. El síntoma clásico: el post-procesado (asignar imagen destacada, indexar, notificar) funciona perfectamente al guardar desde el panel y no hace nada cuando la entrada llega por API. El enganche correcto es **`rest_after_insert_{post_type}`**, que se dispara cuando la entrada está *completamente* creada o actualizada por REST — meta incluidos.

Consecuencia de diseño: si un post-procesado tiene que funcionar por las dos vías, va enganchado a `rest_after_insert_post` **y** a `save_post`, y es idempotente (comprueba primero si el trabajo ya está hecho y sale). Es la misma disciplina que exige cualquier disparador que pueda ejecutarse dos veces.

## Fuentes

- [`rest_after_insert_{$post_type}` — WordPress Developer Resources](https://developer.wordpress.org/reference/hooks/rest_after_insert_this-post_type/)
- [`register_post_meta()` — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/register_post_meta/)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Una integración se ata al identificador estable, no al nombre visible](Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md)
