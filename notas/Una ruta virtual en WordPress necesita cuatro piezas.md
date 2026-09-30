---
type: note
aliases:
  - Rutas virtuales en WordPress
  - add_rewrite_rule
tags:
  - desarrollo
  - wordpress
---

# Una ruta virtual en WordPress necesita cuatro piezas

Servir una URL cuyo contenido vive **en el código del tema** y no en la base de datos —una página institucional, un panel, una vista generada— tiene una ventaja concreta: se despliega con el resto del código, se versiona en git y no hay que entrar al panel de administración para publicarla. El precio es que el texto se edita en un fichero PHP y no en el editor.

WordPress no lo resuelve con una sola llamada. Faltando cualquiera de las cuatro piezas, la ruta falla de una forma distinta y poco evidente:

1. **`add_rewrite_rule()`** en `init`, que traduce el patrón de URL a una consulta interna con una variable propia. Sin esto no hay ruta.
2. **Registrar esa variable en el filtro `query_vars`.** Es el paso que más se olvida: WordPress **descarta cualquier parámetro que no esté declarado**, así que la regla se aplica, la variable se pierde y la plantilla nunca se entera de que la URL se ha usado.
3. **Regenerar las reglas una vez.** Las reglas de reescritura están guardadas en la base de datos, no se leen del código en cada petición: una regla nueva no existe hasta que se hace `flush_rewrite_rules()`. Es una operación cara y la propia documentación avisa de que solo debe usarse cuando es necesario — llamarla en cada carga de página es un error de rendimiento clásico. El patrón correcto es **versionarla con una opción**: si la opción guardada no coincide con el número actual, se regenera y se actualiza. Para forzar el refresco tras cambiar el patrón, se sube el número.
4. **`template_include`** para devolver el fichero del tema… y, dentro, **desactivar el 404 a mano**: `$wp_query->is_404 = false` y `status_header(200)`. WordPress no ha encontrado ninguna entrada para esa URL, así que aunque la página se pinte perfecta en el navegador, **la cabecera sale con `404`** y los buscadores no la indexan nunca. Es el fallo más caro de los cuatro porque es invisible: solo se ve mirando la respuesta cruda.

Lección transferible más allá de WordPress: en un sistema con enrutado propio, **pintar contenido y devolver el estado correcto son dos decisiones separadas**, y la segunda se olvida porque el navegador no la enseña.

## Fuentes

- [`flush_rewrite_rules()` — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/flush_rewrite_rules/)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
