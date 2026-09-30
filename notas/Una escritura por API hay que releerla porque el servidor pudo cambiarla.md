---
type: note
aliases:
  - La respuesta es la verdad, la petición era una propuesta
  - Releer lo escrito por API
tags:
  - desarrollo
  - integracion
  - api
  - wordpress
---

# Una escritura por API hay que releerla porque el servidor pudo cambiarla

Al crear o modificar un recurso por una API REST se asume que lo enviado es lo que queda guardado. No siempre: el servidor **normaliza, completa y resuelve conflictos**, y la respuesta puede diferir de la petición sin que eso sea un error. Dos casos habituales en WordPress ilustran el patrón, y ninguno produce un código de estado distinto de `201`.

**El identificador legible puede llevar sufijo.** Si el `slug` pedido ya existe, WordPress no rechaza la petición: guarda uno distinto añadiéndole un sufijo numérico. La entrada queda publicada, pero en una URL que no es la prevista. Todo lo que se configure después apuntando al `slug` *solicitado* —una entrada fija en la portada, un enlace del menú, una redirección— apunta a una URL que no existe, y el `404` aparece mucho más tarde y lejos de su causa.

**Los campos omitidos se rellenan con valores por defecto que rara vez son los deseados.** El caso claro es la fecha: si no se envía explícitamente, la entrada se sella con la del momento. Al cargar contenido histórico —una recuperación, una migración, un relleno de días anteriores— eso amontona semanas de artículos en la fecha de hoy, y el orden cronológico del sitio queda destruido de una forma que solo se arregla entrada por entrada.

La regla que cubre ambos: **la petición es una propuesta y la respuesta es la verdad**. En consecuencia:

- Leer del cuerpo de la respuesta los campos que el servidor puede haber alterado, y usar **ese** valor en los pasos siguientes, no el que se envió.
- Registrarlo por pantalla o en el diario del proceso, para que la discrepancia se vea en el momento y no se descubra al investigar otra cosa.
- Enviar explícitamente todo campo cuyo valor por defecto no sirva, aunque parezca redundante.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
- [Una integración se ata al identificador estable, no al nombre visible](Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md)
