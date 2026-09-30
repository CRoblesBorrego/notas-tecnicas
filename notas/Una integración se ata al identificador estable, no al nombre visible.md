---
type: note
aliases:
  - Contrato de integración
  - Slug frente a nombre
tags:
  - desarrollo
  - arquitectura
  - integracion
---

# Una integración se ata al identificador estable, no al nombre visible

Cuando dos sistemas se hablan —un flujo de automatización que publica en un gestor de contenidos, un ETL que escribe en una aplicación, cualquier cosa que cruza una API— hay que elegir sobre qué campo se apoya la correspondencia. Casi siempre hay dos candidatos: el **nombre que ve el usuario** y el **identificador técnico** (slug, código, clave).

La regla es apoyarse siempre en el identificador técnico, por un motivo asimétrico: **el nombre visible existe precisamente para poder cambiarlo**. Alguien lo renombra desde el panel para que suene mejor, o lo traduce, o le corrige una tilde. Es una edición legítima, de dos segundos, que ninguna persona asocia con "romper la integración". Y la rotura es silenciosa: el sistema emisor sigue mandando lo mismo con `200 OK`, y el contenido empieza a caer en una categoría por defecto o a no aparecer.

Consecuencias prácticas:

- **El contrato se documenta donde vive el código**, no en la cabeza de quien lo montó. Una cabecera en el fichero de configuración del receptor listando qué identificadores espera y qué campos recibe, con la advertencia de que cambiarlos exige coordinar con el emisor, cuesta cinco líneas.
- **El identificador debe ser inmutable de hecho**, no solo por convención. Si la herramienta permite editar el slug desde la interfaz, ese riesgo sigue ahí — simplemente es mucho menos probable que alguien lo toque sin querer.
- **El receptor tolera lo desconocido.** Si llega un identificador que no reconoce, lo registra y aplica un valor por defecto, en vez de fallar. Un fallo ruidoso en el emisor es preferible a un descarte silencioso, pero peor que ambos es que la publicación entera se caiga por una categoría mal escrita.
- Lo mismo vale para los **nombres de los campos** que viajan en la petición: son contrato, no detalle de implementación. Renombrar un campo del lado receptor es un cambio incompatible aunque el código compile.

Es la versión de integración de una idea más vieja: **lo que se muestra y lo que identifica son cosas distintas**, y confundirlas se paga tarde.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
