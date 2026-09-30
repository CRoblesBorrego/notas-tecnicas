---
type: note
aliases:
  - system.multicall
  - Fuerza bruta amplificada
tags:
  - ciberseguridad
  - wordpress
  - web
---

# XML-RPC multiplica los intentos de contraseña dentro de una sola petición

Contar peticiones es la forma habitual de medir un ataque de fuerza bruta: tantos intentos por minuto, un límite y a correr. El protocolo XML-RPC rompe esa cuenta. Su método `system.multicall` existe para **agrupar varias llamadas en un solo mensaje**, así que una petición HTTP puede transportar cientos de comprobaciones de credenciales.

Las consecuencias prácticas son tres:

- **El límite de peticiones deja de proteger.** Un tope de sesenta peticiones por minuto suena razonable y puede estar dejando pasar decenas de miles de intentos de contraseña.
- **El registro engaña.** En el registro del servidor se ve una línea por petición, con su código de respuesta. XML-RPC responde `200` tanto si la autenticación acierta como si falla —el fallo va dentro del cuerpo XML—, así que ni el volumen ni el código de estado delatan lo que está pasando.
- **La telemetría del proveedor del borde tampoco lo ve.** Cuenta peticiones, que es lo único que puede contar.

Hay un segundo detalle que importa al defenderse: **el `User-Agent` no sirve de filtro**. Las botnets que atacan este endpoint se anuncian con el agente del servicio legítimo que lo usa de verdad, precisamente porque muchas reglas lo dejan pasar por esa cadena. La comprobación válida no es qué dice el agente, sino **si ese servicio está instalado**: si no lo está, todo lo que llegue diciendo serlo es falso, sin excepciones.

Como el endpoint es un protocolo entero y no una función, cerrarlo del todo suele ser preferible a filtrarlo. Antes conviene comprobar quién lo usa: la integración que publica contenido por API REST no lo necesita, pero alguna herramienta antigua sube ficheros por XML-RPC aunque cree el contenido por otra vía, y bloquearlo la deja publicando artículos sin imagen.

La lección general: **cuando un protocolo permite agrupar operaciones, cualquier control basado en contar unidades de transporte deja de medir lo que cree medir.** Lo mismo vale para las peticiones por lotes de una API o para una consulta que acepta varias operaciones anidadas: el límite hay que ponerlo sobre la operación, no sobre el sobre que la lleva.

## Fuentes

- [XML-RPC `system.multicall` — especificación de la extensión](https://mirrors.talideon.com/articles/multicall.html)

## Relacionado

- Operaciones de seguridad (MOC)
- [Publicar un puerto de Docker se salta el cortafuegos del anfitrión](Publicar%20un%20puerto%20de%20Docker%20se%20salta%20el%20cortafuegos%20del%20anfitri%C3%B3n.md)
