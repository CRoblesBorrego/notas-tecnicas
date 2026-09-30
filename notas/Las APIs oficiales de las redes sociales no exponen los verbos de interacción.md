---
type: note
aliases:
  - like follow API
  - automatizar redes sociales
  - Graph API me gusta
tags:
  - desarrollo
  - apis
  - redes-sociales
---

# Las APIs oficiales de las redes sociales no exponen los verbos de interacción

Cualquier encargo de «automatizar redes sociales» llega con la misma lista:
publicar, programar, comentar, dar me gusta, seguir, dejar de seguir, retuitear
y ver historias. Conviene separarla en dos mitades antes de diseñar nada, porque
la frontera no la decide la arquitectura.

**Lo que sí se puede hacer por API oficial**, en prácticamente todas las redes:
publicar texto, imagen y vídeo; programar; leer y responder comentarios **del
contenido propio**; leer menciones y mensajes; leer métricas propias; borrar lo
publicado. Es el caso de uso para el que estas APIs existen y cubre la mayor
parte de las horas de trabajo manual.

**Lo que no**, con dos matices distintos:

- **Meta no expone ningún endpoint de «me gusta» ni de «seguir»** en la Graph
  API, y no lo ha expuesto nunca. No es cuestión de plan ni de revisión de la
  app: la operación no existe en la superficie pública. Lo mismo con ver
  historias ajenas; solo se leen métricas de las propias mientras están activas.
- **X sí los tiene, pero se han ido cerrando.** Los endpoints de «me gusta» y de
  seguimiento salieron del plan gratuito en **agosto de 2025**, y en **febrero de
  2026** se restringieron las respuestas, menciones y citas programáticas.

De ahí salen dos consecuencias de producto:

1. **Los verbos de interacción no existen como funcionalidad multiplataforma.**
   Se pueden construir para una red pagando y no se pueden construir para otra a
   ningún precio. Un panel con un botón que solo funciona en una de las cuentas
   conectadas es peor que no tener el botón.
2. **Donde existen, actúan como el titular sobre su propia cuenta.** No hay
   ninguna vía legítima para repartir N interacciones entre M cuentas sobre
   contenido de terceros: eso es interacción artificial, lo prohíben los términos
   de todas las plataformas y es justo lo que persiguen sus sistemas antiabuso.
   Lo que se construye para esquivarlos —huellas de navegador únicas, proxies
   rotativos, curvas de ratón, calentamiento de cuentas nuevas— es la definición
   del problema, no la solución.

La lección transferible: **antes de diseñar sobre una API de terceros hay que
separar lo que la API permite de lo que el producto imagina**, y hacerlo por
escrito y con fecha. Es la restricción más dura del diseño, cambia varias veces
al año y no se negocia con más ingeniería.

## Fuentes

- [X API changelog](https://docs.x.com/changelog)
- [Instagram Platform — Graph API](https://developers.facebook.com/docs/instagram-platform)

## Relacionado

- [Una integración se ata al identificador estable, no al nombre visible](Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md)
- [Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar](Una%20fuente%20externa%20con%20cuota%20debe%20fallar%20hacia%20continuar%2C%20no%20hacia%20abortar.md)
- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
