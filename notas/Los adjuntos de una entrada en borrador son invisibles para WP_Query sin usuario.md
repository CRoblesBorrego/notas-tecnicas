---
type: note
aliases:
  - Adjuntos de borradores en WordPress
  - post_status inherit
  - Attachment invisible en CLI
tags:
  - desarrollo
  - wordpress
  - automatizacion
---

# Los adjuntos de una entrada en borrador son invisibles para WP_Query sin usuario

Un script que crea una entrada en borrador y le cuelga imágenes, y que se quiere **idempotente** —volver a ejecutarlo actualiza en lugar de duplicar—, busca cada adjunto por su slug antes de crearlo. Esa búsqueda devuelve **cero** y el script duplica todos los adjuntos en cada pasada, aunque la fila esté en la base de datos con el `post_name` exacto.

La causa no es el slug. Los adjuntos se guardan con `post_status = 'inherit'`: **no tienen estado propio, heredan el del padre**. Cuando `WP_Query` resuelve `inherit`, no se limita a comparar la columna; comprueba que el estado heredado sea legible **para el usuario actual**. Un proceso de línea de comandos, una tarea de cron o un enganche sin sesión no tienen usuario actual, así que el adjunto de un borrador queda fuera del resultado. La entrada existe, es correcta y no se puede encontrar.

Comportamiento observado con el mismo slug y la misma consulta:

| Contexto | ¿Encuentra el adjunto? |
|---|---|
| Sin usuario, padre en borrador | no |
| Sin usuario, `post_status => 'any'` | no |
| `wp_set_current_user()` con un administrador | **sí** |
| Sin usuario, padre publicado | **sí** |

Tres consecuencias prácticas:

- **`post_status => 'any'` no salva nada.** Suena a comodín y no lo es para `inherit`: sigue pasando por la comprobación de permisos.
- **La solución es fijar el usuario**, no rodear la consulta. `wp_set_current_user()` con una cuenta que pueda leer borradores, al principio del script, y el resto del código funciona igual en el panel y fuera de él.
- **Bajar a SQL directo funciona pero miente.** `$wpdb->get_var()` encuentra la fila porque no aplica ninguna regla de visibilidad — precisamente por eso no es la respuesta: el día que ese script corra sobre contenido que sí debe filtrarse, el filtro ya no está.

La lección general, más allá de WordPress: **cuando un estado se hereda, la consulta que lo resuelve suele arrastrar también las reglas de permisos del padre**. Un contexto sin identidad no es un contexto con todos los permisos; es un contexto con ninguno. Los scripts de automatización se escriben pensando que "no hay usuario" equivale a "modo administrador", y casi siempre es al revés.

## Fuentes

- [`WP_Query` — WordPress Developer Resources](https://developer.wordpress.org/reference/classes/wp_query/)
- Comprobado sobre WordPress con PHP 8.3, ejecutando el script como usuario del servidor web.

## Relacionado

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
