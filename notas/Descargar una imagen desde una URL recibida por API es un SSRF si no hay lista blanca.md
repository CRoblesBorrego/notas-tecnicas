---
type: note
aliases:
  - Sideload de imágenes
  - SSRF por URL de imagen
tags:
  - desarrollo
  - wordpress
  - ciberseguridad
  - integracion
---

# Descargar una imagen desde una URL recibida por API es un SSRF si no hay lista blanca

Patrón muy común en integraciones de publicación: el cliente externo manda la entrada con una URL de imagen en un campo, y el servidor la **descarga y la guarda** como imagen destacada (`media_sideload_image` en WordPress, cualquier `fetch` seguido de un guardado en otro sitio).

Escrito así, es un SSRF de manual: se le está dando a un tercero la capacidad de hacer que **el servidor emita peticiones a la URL que él elija**, desde dentro de la red, con la identidad de la máquina. Los destinos interesantes no son webs públicas: son el servicio de metadatos de la instancia, un panel de administración escuchando solo en local, una base de datos sin autenticación en la red interna. Y como la respuesta se guarda como adjunto, a menudo el atacante **también puede leerla**.

La defensa mínima es una **lista blanca de dominios**, no una lista negra ni una validación de formato:

1. Validar que la URL es sintácticamente aceptable y con esquema HTTP(S).
2. Extraer el host y compararlo contra los dominios permitidos, aceptando subdominios solo con una comprobación de sufijo bien escrita: `host === permitido || host.termina_en('.' + permitido)`. Sin ese punto, `permitidomalicioso.com` pasa el filtro de `terminaEn('permitido.com')`.
3. Si no está en la lista, **no descargar y seguir adelante**: la entrada se publica sin imagen. Un fallo de imagen no debe tumbar la publicación.

Lo que la lista blanca no cubre, y conviene saber: **redirecciones** (el dominio permitido puede responder `302` hacia una IP interna, así que hay que revalidar en cada salto o prohibirlas), **resolución DNS a direcciones privadas**, y el tamaño de la descarga, que sin límite es una vía de agotamiento de disco.

El principio general: cualquier campo que un cliente controle y que acabe convertido en **una petición saliente del servidor** es superficie de ataque, aunque parezca dato inerte. Una URL no es una cadena de texto; es una instrucción.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
