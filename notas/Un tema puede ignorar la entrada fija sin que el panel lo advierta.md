---
type: note
aliases:
  - ignore_sticky_posts
  - Entrada fija en WordPress
  - Sticky posts
tags:
  - desarrollo
  - wordpress
---

# Un tema puede ignorar la entrada fija sin que el panel lo advierta

Marcar una entrada como **fija** (*sticky*) es el gesto que ofrece WordPress para decir "esto va primero". El panel muestra la casilla, la guarda, y la lista de entradas etiqueta la pieza como destacada. Nada de eso garantiza que se vea distinta en la portada.

La portada de un tema a medida casi nunca usa la consulta principal: monta la suya con `new WP_Query()`. Y en esa consulta es habitual encontrar `'ignore_sticky_posts' => true`, un parámetro que se copia sin pensar porque en las **rejillas secundarias** es lo correcto — sin él, la entrada fija se cuela en todos los módulos de la página y aparece repetida tres veces. El problema es que suele acabar también en la consulta de cabecera, la que elige la historia principal, y ahí es justo lo contrario de lo que se quiere.

El resultado es una discrepancia silenciosa: **el editor cree que ha destacado algo y el orden real sigue siendo por fecha**. Nadie recibe un aviso, porque desde el punto de vista de WordPress todo funciona.

Dónde duele de verdad: en una web donde publica un proceso automático. Una pieza de fondo, trabajada durante días, ocupa la portada solo hasta la siguiente tanda automática —unas horas— y después baja como una noticia más. La marca de fija existía precisamente para evitar eso y no estaba conectada a nada.

Regla de comprobación, aplicable a cualquier gestor de contenidos con plantillas propias: **una marca editorial solo es real si alguna consulta la lee**. Antes de dar por buena una función de "destacar", hay que buscar el parámetro en las plantillas, no en el panel. Y si la consulta de cabecera y las de relleno necesitan comportamientos opuestos, se configuran por separado: quitar `ignore_sticky_posts` de la primera y dejarlo en las segundas, excluyendo además de estas los identificadores ya mostrados.

Corolario más amplio: los campos que el panel enseña y las plantillas ignoran son **deuda de interfaz**. Prometen un control que no existe, y el coste no lo paga quien programa el tema sino quien lo usa a diario.

## Fuentes

- [Sticky post parameters, `WP_Query` — WordPress Developer Resources](https://developer.wordpress.org/reference/classes/wp_query/)

## Relacionado

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
