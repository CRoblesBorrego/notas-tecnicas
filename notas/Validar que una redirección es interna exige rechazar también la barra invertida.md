---
type: note
aliases:
  - Redirección abierta tras el login
  - Open redirect
tags:
  - desarrollo
  - seguridad
  - web
---

# Validar que una redirección es interna exige rechazar también la barra invertida

El patrón típico de «volver a donde estabas» tras iniciar sesión lleva la ruta de destino en la URL (`/login?siguiente=/panel`) y, al acabar, redirige allí. Si el valor no se valida, es una **redirección abierta**: un enlace legítimo del propio dominio que, tras un login real, deja a la víctima en una web ajena preparada para pedirle otra vez la contraseña.

La comprobación que todo el mundo escribe primero es «empieza por `/` y no por `//`», para descartar tanto las URL absolutas como las relativas al protocolo (`//malo.example`). No basta: **los navegadores tratan la barra invertida como una barra normal** en las URL `http(s)`, así que `/\malo.example` pasa el filtro y el navegador lo interpreta como `//malo.example`. Tampoco hay que dejar pasar caracteres de control: un tabulador o un salto de línea dentro de la ruta se eliminan al analizarla y pueden recomponer el mismo truco.

La regla que funciona es una lista blanca estricta, en una única función compartida que usen todos los sitios que redirigen:

- Debe empezar por `/` y el segundo carácter no puede ser ni `/` ni `\` (`^\/(?![\/\\])`).
- Sin ningún carácter de control (`\x00-\x1f`, `\x7f`).
- Cualquier otra cosa devuelve la ruta por defecto (`/`), sin mensaje de error que ayude a afinar el ataque.

Las variantes codificadas (`%2F%2F`, `%5C`) no son un problema adicional si el valor se decodifica **una sola vez** antes de validarlo: un `%2F` que sobrevive a esa decodificación queda dentro de la ruta como texto y el analizador de URL no lo convierte en separador.

## Fuentes

- [Unvalidated Redirects and Forwards Cheat Sheet — OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Unvalidated_Redirects_and_Forwards_Cheat_Sheet.html)
- [URL Standard — WHATWG](https://url.spec.whatwg.org/) (en esquemas especiales, `\` se trata como `/`)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [El middleware no puede redirigir al login las rutas que consume un cliente no navegador](El%20middleware%20no%20puede%20redirigir%20al%20login%20las%20rutas%20que%20consume%20un%20cliente%20no%20navegador.md)
