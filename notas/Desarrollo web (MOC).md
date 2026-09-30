---
type: moc
title: Desarrollo web (MOC)
aliases:
  - MOC Desarrollo web
  - MOC Web
tags:
  - moc
  - desarrollo
  - web
---

# Desarrollo web — Mapa de contenido

Modos de fallo del desarrollo web que **no dan error**: la petición devuelve `201`, la página se ve bien, el guardado funciona desde el panel. El daño aparece días después, cuando alguien nota que un campo está vacío o que una URL no se indexa. Van juntas aquí las de gestor de contenidos y las de aplicación con renderizado en servidor, porque el patrón que comparten es el mismo.

## Integración por API con un gestor de contenidos

- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md) — sin `show_in_rest` el campo se descarta sin avisar, y en `save_post` los meta enviados por REST todavía no existen.
- [Descargar una imagen desde una URL recibida por API es un SSRF si no hay lista blanca](Descargar%20una%20imagen%20desde%20una%20URL%20recibida%20por%20API%20es%20un%20SSRF%20si%20no%20hay%20lista%20blanca.md) — una URL controlada por el cliente que el servidor visita es una instrucción, no un dato.
- [Una integración se ata al identificador estable, no al nombre visible](Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md) — el nombre existe para cambiarlo; el contrato se apoya en el slug y se documenta junto al código.
- [Una ruta virtual en WordPress necesita cuatro piezas](Una%20ruta%20virtual%20en%20WordPress%20necesita%20cuatro%20piezas.md) — regla, variable de consulta, regeneración versionada y plantilla que además desactive el `404`.
- [Una escritura por API hay que releerla porque el servidor pudo cambiarla](Una%20escritura%20por%20API%20hay%20que%20releerla%20porque%20el%20servidor%20pudo%20cambiarla.md) — el `slug` con sufijo por colisión y la fecha por defecto que amontona el contenido histórico en el día de hoy.
- [La cabecera Host decide qué sitio responde cuando un servidor aloja varios](La%20cabecera%20Host%20decide%20qu%C3%A9%20sitio%20responde%20cuando%20un%20servidor%20aloja%20varios.md) — saltarse el proxy obliga a poner a mano lo que ponía él; sin la cabecera se lee y se escribe en el sitio equivocado con `200`.
- [Un 403 puede estar rechazando al cliente y no a la clave](Un%20403%20puede%20estar%20rechazando%20al%20cliente%20y%20no%20a%20la%20clave.md) — el filtro perimetral mira el agente de usuario antes de que la API vea la credencial.

## Carga de contenido por script

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md) — la API interna sin autenticación HTTP, y por qué los límites de subida dejan de aplicar.
- [Los adjuntos de una entrada en borrador son invisibles para WP_Query sin usuario](Los%20adjuntos%20de%20una%20entrada%20en%20borrador%20son%20invisibles%20para%20WP_Query%20sin%20usuario.md) — `inherit` arrastra los permisos del padre, y un proceso sin sesión no tiene ninguno.
- [Un tema puede ignorar la entrada fija sin que el panel lo advierta](Un%20tema%20puede%20ignorar%20la%20entrada%20fija%20sin%20que%20el%20panel%20lo%20advierta.md) — una marca editorial solo es real si alguna consulta la lee.
- [Un PDF nativo se lee mejor por su capa de texto e imágenes incrustadas que rasterizando](Un%20PDF%20nativo%20se%20lee%20mejor%20por%20su%20capa%20de%20texto%20e%20im%C3%A1genes%20incrustadas%20que%20rasterizando.md) — texto sin reconocimiento óptico e imágenes en su resolución original, sin binarios externos.
- [Chromium sin cabeza necesita un perfil propio en cada invocación para capturar](Chromium%20sin%20cabeza%20necesita%20un%20perfil%20propio%20en%20cada%20invocaci%C3%B3n%20para%20capturar.md) — el bloqueo del perfil convierte "no pude arrancar" en un código de salida 0.
- [Al cambiar de tema de WordPress sobran casi todas las miniaturas de uploads](Al%20cambiar%20de%20tema%20de%20WordPress%20sobran%20casi%20todas%20las%20miniaturas%20de%20uploads.md) — conservar solo los tamaños que se usan, reducir originales y reescribir los metadatos para que `srcset` no anuncie ficheros borrados.

## Maquetación

- [Centrar contenido que desborda lo vuelve inalcanzable por un lado](Centrar%20contenido%20que%20desborda%20lo%20vuelve%20inalcanzable%20por%20un%20lado.md) — `justify-content: safe center`: lo que se sale por la izquierda de un contenedor centrado no se alcanza desplazando.
- [Centrar con margin auto en un ancho impar deja el texto a medio píxel y borroso](Centrar%20con%20margin%20auto%20en%20un%20ancho%20impar%20deja%20el%20texto%20a%20medio%20p%C3%ADxel%20y%20borroso.md) — `round()` en el margen, interlineados enteros, iconos a escala 1:1, y el límite del escalado de pantalla al 125 %.

## Aplicación con renderizado en servidor

- [El middleware no puede redirigir al login las rutas que consume un cliente no navegador](El%20middleware%20no%20puede%20redirigir%20al%20login%20las%20rutas%20que%20consume%20un%20cliente%20no%20navegador.md) — `401` frente a `302`, igualdad exacta frente a prefijo, y el renombrado de `middleware` a `proxy` en Next.js 16.
- [Un control que depende del cliente debe reservar su hueco hasta montarse](Un%20control%20que%20depende%20del%20cliente%20debe%20reservar%20su%20hueco%20hasta%20montarse.md) — discrepancia de hidratación, hueco en vez de `null` y el parpadeo del tema.
- [Construir la imagen de una app no debe requerir la base de datos](Construir%20la%20imagen%20de%20una%20app%20no%20debe%20requerir%20la%20base%20de%20datos.md) — el paso de recolección de datos de página importa todos los módulos de ruta, y ahí revienta lo que se construye al importar.
- [Validar que una redirección es interna exige rechazar también la barra invertida](Validar%20que%20una%20redirecci%C3%B3n%20es%20interna%20exige%20rechazar%20tambi%C3%A9n%20la%20barra%20invertida.md) — `/\malo.example` pasa el filtro de «empieza por `/` y no por `//`»; lista blanca en una única función.
- [Un select no controlado no se entera de que los datos del servidor cambiaron](Un%20select%20no%20controlado%20no%20se%20entera%20de%20que%20los%20datos%20del%20servidor%20cambiaron.md) — `defaultValue` solo vale al montar; tras refrescar, `key` con el dato del servidor.
- [En una unión de zod con coerce.date el null se convierte en 1970](En%20una%20uni%C3%B3n%20de%20zod%20con%20coerce.date%20el%20null%20se%20convierte%20en%201970.md) — la primera opción que acepta gana, y `new Date(null)` es una fecha válida.

## Endurecer y operar el servidor

- [WordPress descarta la escritura directa cuando PHP corre como root](WordPress%20descarta%20la%20escritura%20directa%20cuando%20PHP%20corre%20como%20root.md) — `FS_METHOD` cae a `ftpsockets` por seguridad y el panel pide credenciales de FTP que no existen; la salida es actualizar por línea de órdenes.
- [XML-RPC multiplica los intentos de contraseña dentro de una sola petición](XML-RPC%20multiplica%20los%20intentos%20de%20contrase%C3%B1a%20dentro%20de%20una%20sola%20petici%C3%B3n.md) — `system.multicall` mete cientos de pruebas en una sola solicitud, así que limitar por peticiones no frena nada.
- [Los recursos externos de una página se inventarían desde el DOM, no con una expresión regular](Los%20recursos%20externos%20de%20una%20p%C3%A1gina%20se%20inventar%C3%ADan%20desde%20el%20DOM%2C%20no%20con%20una%20expresi%C3%B3n%20regular.md) — antes de escribir una CSP hay que saber qué carga la página de verdad, y eso no se saca con una expresión regular sobre el HTML.
- [Un sitio virtual de Apache se vuelve el predeterminado por el orden alfabético de su fichero](Un%20sitio%20virtual%20de%20Apache%20se%20vuelve%20el%20predeterminado%20por%20el%20orden%20alfab%C3%A9tico%20de%20su%20fichero.md) — el primero que se carga responde a todo lo que no tiene dueño; comparar `apache2ctl -S` antes y después de activar un sitio.
- [Un WordPress comprometido se reconstruye con código oficial en vez de limpiarse](Un%20WordPress%20comprometido%20se%20reconstruye%20con%20c%C3%B3digo%20oficial%20en%20vez%20de%20limpiarse.md) — cómo se camuflan las puertas traseras, sumas de verificación del núcleo y qué se puede reutilizar de la copia.

## Hilo conductor

Las tres causas que se repiten: **el descarte silencioso** (el marco ignora lo que no reconoce en vez de protestar), **el orden de ejecución** (el enganche se dispara antes de que exista el dato que necesita) y **la confusión entre lo que se pinta y lo que se responde** (la página se ve, la cabecera dice otra cosa). Ninguna la detecta el compilador y casi ninguna la detecta una prueba manual desde el navegador: hacen falta pruebas que miren la respuesta cruda y que recorran la vía de la API, no solo la del panel.

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md) — lo que pasa alrededor de la aplicación.
- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md) — lo que llama a estas APIs desde el otro lado.
- [Índice de Notas](../README.md)
