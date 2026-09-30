---
type: note
aliases:
  - Inventario previo a una CSP
  - Regex sobre HTML
tags:
  - desarrollo
  - ciberseguridad
  - web
---

# Los recursos externos de una página se inventarían desde el DOM, no con una expresión regular

Antes de publicar una `Content-Security-Policy` hay que saber qué carga la página de fuera: cada dominio ausente de la política es un recurso que deja de cargarse. La tentación es descargar el HTML y buscar con una expresión regular algo como `(?:src|href)="https?://…`.

Esa comprobación **da un falso negativo con toda naturalidad**, por tres motivos:

1. **Los atributos no tienen orden.** `<link rel="stylesheet" href="…">` y `<link href="…" rel="stylesheet">` son idénticos para el navegador, pero un patrón que exija `rel` antes de `href` solo ve el primero.
2. **No todo recurso está en el HTML inicial.** Lo que inyecta un script en tiempo de ejecución, o lo que se carga desde una `@import` o un `url()` dentro de una hoja de estilos, no aparece nunca en el documento descargado.
3. **Una hoja de estilos externa arrastra más peticiones.** Un archivo de fuentes tipográficas lo pide el CSS, no el HTML: el dominio de las hojas de estilo y el de los ficheros de fuente suelen ser **distintos**, y el segundo no está en el HTML por ningún lado.

El inventario fiable se hace **en el navegador y sobre la página ya cargada**, consultando el DOM y comparando cada origen con el de la propia página. Y aun así hace falta el segundo paso: **abrir la página con la política ya puesta y leer la consola**, porque cada bloqueo de CSP deja un mensaje que dice exactamente qué directiva ha fallado y qué recurso se ha perdido.

Lo transferible no es el detalle de la CSP. Es que **analizar el texto de un formato estructurado es una aproximación, no una medición**: el HTML lo interpreta un analizador sintáctico, y cualquier atajo basado en buscar cadenas acierta en los casos frecuentes y falla en los legítimos que no se habían previsto. Cuando el coste de equivocarse es romper la página en producción, el inventario se toma del sistema que de verdad la interpreta.

## Fuentes

- [Content Security Policy — MDN](https://developer.mozilla.org/es/docs/Web/HTTP/CSP)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- Operaciones de seguridad (MOC)
