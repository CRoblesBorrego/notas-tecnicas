---
type: note
aliases:
  - Alojamiento virtual por nombre
  - Cabecera Host en peticiones al bucle local
tags:
  - desarrollo
  - web
  - integracion
  - http
---

# La cabecera Host decide qué sitio responde cuando un servidor aloja varios

En un alojamiento virtual por nombre, un mismo servidor web —o un mismo contenedor— sirve varios sitios distintos por **una sola dirección y un solo puerto**. Lo que decide cuál de ellos responde no es la dirección ni el puerto: es la **cabecera `Host` de la petición**. En el tráfico normal la pone el navegador a partir de la URL y el proxy inverso la propaga, así que nunca se piensa en ella.

Se vuelve visible en cuanto un script habla **directamente con el bucle local**, saltándose el proxy: ahí no hay nada que rellene la cabecera por nosotros. Y el fallo es del tipo peor, porque **no da error**. Sin `Host` correcto, el servidor entrega el sitio configurado por defecto, que responde `200` con contenido plausible. Una lectura devuelve datos —los de otro sitio— y una escritura se aplica sobre la instalación equivocada. Todo parece haber funcionado.

De ahí tres consecuencias prácticas:

- Cualquier cliente que ataque el bucle local contra un servidor multi-sitio debe **fijar `Host` explícitamente** en cada petición, igual que fija la autenticación. No es un detalle opcional de configuración.
- Las **sondas de salud** tienen el mismo problema: comprobar el puerto sin cabecera verifica que el sitio por defecto está vivo, no el que interesa. Una sonda que siempre da verde por el sitio equivocado es peor que no tenerla.
- Cuando dos instalaciones conviven así, conviene que la credencial y la cabecera **viajen juntas en la misma estructura de configuración**. Separarlas invita a la combinación cruzada —las credenciales de un sitio con el `Host` de otro—, que según los permisos da un `401` desconcertante o, peor, escribe donde no debía.

Es un caso particular de un patrón más amplio: **saltarse una capa intermedia obliga a reproducir a mano todo lo que esa capa hacía en silencio**.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- [Una escritura por API hay que releerla porque el servidor pudo cambiarla](Una%20escritura%20por%20API%20hay%20que%20releerla%20porque%20el%20servidor%20pudo%20cambiarla.md)
