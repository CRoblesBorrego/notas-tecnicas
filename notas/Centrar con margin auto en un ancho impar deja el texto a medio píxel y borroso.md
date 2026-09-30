---
type: note
aliases:
  - Texto borroso por subpíxel
  - margin auto medio píxel
tags:
  - desarrollo
  - css
  - diseño
---

# Centrar con margin auto en un ancho impar deja el texto a medio píxel y borroso

Un contenedor de 1140 px centrado con `margin: 0 auto` en una ventana de 1440 px parece inocente. Pero con la barra de desplazamiento clásica de Windows, que ocupa unos 15 px, el ancho útil es 1425 px, y el margen queda en **142,5 px**. Todo el contenido empieza en mitad de un píxel. El navegador reparte cada trazo de letra y cada borde de SVG entre dos píxeles, y el resultado es un desenfoque leve y uniforme. Sobre blanco o gris oscuro apenas se nota, pero sobre un color saturado, como rojo puro, salta a la vista.

Los desfases verticales se acumulan igual. Un logo con `height: auto` que resulta de 89,17 px, o un `line-height: 1.2` sobre 36 px (43,2 px), empujan medio píxel a todo lo que viene debajo.

La corrección es de medir, no de adivinar. Hay que recorrer con `getBoundingClientRect()` los elementos afectados y buscar coordenadas con decimales. Para arreglarlo:

- **Redondear el centrado**: `margin-left: max(0px, round(down, (100% - var(--ancho)) / 2, 1px))`. Va dentro de `@supports`, con `margin: auto` como respaldo.
- **Usar interlineados en píxeles enteros** en los titulares, en lugar de multiplicadores que den decimales.
- **Dar alto fijo a las imágenes de maqueta** (logos) en lugar de `height: auto`.
- **Mostrar los iconos SVG a escala 1:1.** Un `viewBox` de 76×56 mostrado a 74×54 escala todo un 97 %, y ningún borde recto coincide ya con un píxel. Los radios y grosores de trazo también deben ser enteros.

Un límite importante: con el **escalado de pantalla de Windows al 125 % o 150 %**, un píxel CSS equivale a 1,25 o 1,5 píxeles físicos, y ninguna alineación en CSS lo evita del todo. La pista para distinguirlo es que al ampliar el navegador el desenfoque desaparece. En ese caso, lo que queda es de diseño: texto pequeño y fino sobre un rojo saturado «vibra» y el ojo lo percibe borroso. Un rojo algo menos saturado con texto blanco en seminegrita lo resuelve.

## Fuentes

- [round() — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/round)
- [Element.getBoundingClientRect() — MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)

## Relacionado

- [Centrar contenido que desborda lo vuelve inalcanzable por un lado](Centrar%20contenido%20que%20desborda%20lo%20vuelve%20inalcanzable%20por%20un%20lado.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
