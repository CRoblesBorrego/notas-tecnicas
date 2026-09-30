---
type: note
aliases:
  - justify-content safe center
  - Barra deslizable centrada
tags:
  - desarrollo
  - css
  - accesibilidad
---

# Centrar contenido que desborda lo vuelve inalcanzable por un lado

Una barra de secciones centrada con `justify-content: center` y con `overflow-x: auto` para que se deslice en pantallas pequeñas parece la combinación correcta: bonita cuando cabe, deslizable cuando no. No lo es.

Al centrar, el contenido que sobra **se reparte por los dos lados**. Lo que se sale por la derecha se alcanza desplazando; lo que se sale por la izquierda **no se alcanza nunca**, porque `scrollLeft` no admite valores negativos y el navegador lo trunca a cero. Los elementos existen en el árbol, ocupan espacio, son visibles para un lector de pantalla y para el buscador, pero ningún gesto los trae a la pantalla.

En un caso real, las dos primeras secciones de un menú quedaban en −122 y −26 píxeles: invisibles y sin forma de llegar a ellas, mientras la barra sí se deslizaba hacia la derecha y aparentaba funcionar. Ese es el motivo de que el fallo sobreviva a las pruebas: se comprueba que la barra se mueve, no que se llegue a todo.

La solución está en el propio estándar. La palabra clave **`safe`** existe exactamente para esto:

```css
justify-content: safe center;
```

Centra mientras el contenido quepa y, en cuanto desborda, se comporta como `flex-start`, que es la alineación desde la que todo es alcanzable. Se le puede anteponer un `justify-content: center` como respaldo para navegadores antiguos, y añadir una consulta de medios que fuerce `flex-start` en móvil si se quiere garantía total.

Dos ideas que valen para cualquier contenedor deslizable:

**Un desplazamiento sin pista visual no existe para el usuario.** Si nada indica que hay más contenido —un degradado en el borde, un elemento cortado a media vista— la mayoría no lo descubre. Conviene dejar siempre un indicio.

**Comprobar que algo se desplaza no es comprobar que se llega a todo.** Son dos verificaciones distintas, y la segunda es la que importa.

## Fuentes

- [Alineación segura: `safe` y `unsafe` — CSS Box Alignment, W3C](https://www.w3.org/TR/css-align-3/#overflow-values)

## Relacionado

- Un elemento con proporción fija deriva su ancho de la altura
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
