# Notas técnicas · Technical notes

Lecciones de desarrollo web, despliegue con contenedores y automatización con n8n, sacadas de proyectos reales y escritas
como conocimiento general: **una idea por nota**, con un título que ya es la conclusión, el modo de fallo, cómo se arregla y
las fuentes. No contienen datos de clientes ni de infraestructura.

*Lessons on web development, container deployment and n8n automation, distilled from real projects and written as
general knowledge: one idea per note, a title that states the conclusion, the failure mode, the fix and sources.
Notes are in Spanish.*

Cada nota empieza con metadatos (tipo, etiquetas) y termina con **Fuentes** y **Relacionado**. Los mapas de contenido
(MOC) agrupan las notas por tema y explican el hilo que las une.

### [Desarrollo web](notas/Desarrollo%20web%20%28MOC%29.md)

Modos de fallo del desarrollo web que no dan error: APIs de CMS, aplicaciones con renderizado en servidor y endurecimiento.

- [Escribir campos meta por la REST API de WordPress falla de dos maneras silenciosas](notas/Escribir%20campos%20meta%20por%20la%20REST%20API%20de%20WordPress%20falla%20de%20dos%20maneras%20silenciosas.md)
- [Descargar una imagen desde una URL recibida por API es un SSRF si no hay lista blanca](notas/Descargar%20una%20imagen%20desde%20una%20URL%20recibida%20por%20API%20es%20un%20SSRF%20si%20no%20hay%20lista%20blanca.md)
- [Una integración se ata al identificador estable, no al nombre visible](notas/Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md)
- [Una ruta virtual en WordPress necesita cuatro piezas](notas/Una%20ruta%20virtual%20en%20WordPress%20necesita%20cuatro%20piezas.md)
- [Una escritura por API hay que releerla porque el servidor pudo cambiarla](notas/Una%20escritura%20por%20API%20hay%20que%20releerla%20porque%20el%20servidor%20pudo%20cambiarla.md)
- [La cabecera Host decide qué sitio responde cuando un servidor aloja varios](notas/La%20cabecera%20Host%20decide%20qu%C3%A9%20sitio%20responde%20cuando%20un%20servidor%20aloja%20varios.md)
- [Un 403 puede estar rechazando al cliente y no a la clave](notas/Un%20403%20puede%20estar%20rechazando%20al%20cliente%20y%20no%20a%20la%20clave.md)
- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](notas/Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Los adjuntos de una entrada en borrador son invisibles para WP_Query sin usuario](notas/Los%20adjuntos%20de%20una%20entrada%20en%20borrador%20son%20invisibles%20para%20WP_Query%20sin%20usuario.md)
- [Un tema puede ignorar la entrada fija sin que el panel lo advierta](notas/Un%20tema%20puede%20ignorar%20la%20entrada%20fija%20sin%20que%20el%20panel%20lo%20advierta.md)
- [Un PDF nativo se lee mejor por su capa de texto e imágenes incrustadas que rasterizando](notas/Un%20PDF%20nativo%20se%20lee%20mejor%20por%20su%20capa%20de%20texto%20e%20im%C3%A1genes%20incrustadas%20que%20rasterizando.md)
- [Chromium sin cabeza necesita un perfil propio en cada invocación para capturar](notas/Chromium%20sin%20cabeza%20necesita%20un%20perfil%20propio%20en%20cada%20invocaci%C3%B3n%20para%20capturar.md)
- [Al cambiar de tema de WordPress sobran casi todas las miniaturas de uploads](notas/Al%20cambiar%20de%20tema%20de%20WordPress%20sobran%20casi%20todas%20las%20miniaturas%20de%20uploads.md)
- [Centrar contenido que desborda lo vuelve inalcanzable por un lado](notas/Centrar%20contenido%20que%20desborda%20lo%20vuelve%20inalcanzable%20por%20un%20lado.md)
- [Centrar con margin auto en un ancho impar deja el texto a medio píxel y borroso](notas/Centrar%20con%20margin%20auto%20en%20un%20ancho%20impar%20deja%20el%20texto%20a%20medio%20p%C3%ADxel%20y%20borroso.md)
- [El middleware no puede redirigir al login las rutas que consume un cliente no navegador](notas/El%20middleware%20no%20puede%20redirigir%20al%20login%20las%20rutas%20que%20consume%20un%20cliente%20no%20navegador.md)
- [Un control que depende del cliente debe reservar su hueco hasta montarse](notas/Un%20control%20que%20depende%20del%20cliente%20debe%20reservar%20su%20hueco%20hasta%20montarse.md)
- [Construir la imagen de una app no debe requerir la base de datos](notas/Construir%20la%20imagen%20de%20una%20app%20no%20debe%20requerir%20la%20base%20de%20datos.md)
- [Validar que una redirección es interna exige rechazar también la barra invertida](notas/Validar%20que%20una%20redirecci%C3%B3n%20es%20interna%20exige%20rechazar%20tambi%C3%A9n%20la%20barra%20invertida.md)
- [Un select no controlado no se entera de que los datos del servidor cambiaron](notas/Un%20select%20no%20controlado%20no%20se%20entera%20de%20que%20los%20datos%20del%20servidor%20cambiaron.md)
- [En una unión de zod con coerce.date el null se convierte en 1970](notas/En%20una%20uni%C3%B3n%20de%20zod%20con%20coerce.date%20el%20null%20se%20convierte%20en%201970.md)
- [WordPress descarta la escritura directa cuando PHP corre como root](notas/WordPress%20descarta%20la%20escritura%20directa%20cuando%20PHP%20corre%20como%20root.md)
- [XML-RPC multiplica los intentos de contraseña dentro de una sola petición](notas/XML-RPC%20multiplica%20los%20intentos%20de%20contrase%C3%B1a%20dentro%20de%20una%20sola%20petici%C3%B3n.md)
- [Los recursos externos de una página se inventarían desde el DOM, no con una expresión regular](notas/Los%20recursos%20externos%20de%20una%20p%C3%A1gina%20se%20inventar%C3%ADan%20desde%20el%20DOM%2C%20no%20con%20una%20expresi%C3%B3n%20regular.md)
- [Un sitio virtual de Apache se vuelve el predeterminado por el orden alfabético de su fichero](notas/Un%20sitio%20virtual%20de%20Apache%20se%20vuelve%20el%20predeterminado%20por%20el%20orden%20alfab%C3%A9tico%20de%20su%20fichero.md)
- [Un WordPress comprometido se reconstruye con código oficial en vez de limpiarse](notas/Un%20WordPress%20comprometido%20se%20reconstruye%20con%20c%C3%B3digo%20oficial%20en%20vez%20de%20limpiarse.md)
### [Despliegue con contenedores](notas/Despliegue%20con%20contenedores%20%28MOC%29.md)

Lo que se rompe entre el `docker build` del portátil y un servicio que aguanta en producción.

- [Construir la imagen de una app no debe requerir la base de datos](notas/Construir%20la%20imagen%20de%20una%20app%20no%20debe%20requerir%20la%20base%20de%20datos.md)
- [Publicar un puerto de Docker se salta el cortafuegos del anfitrión](notas/Publicar%20un%20puerto%20de%20Docker%20se%20salta%20el%20cortafuegos%20del%20anfitri%C3%B3n.md)
- [Una sonda de salud comprueba las dependencias, no que el proceso siga vivo](notas/Una%20sonda%20de%20salud%20comprueba%20las%20dependencias%2C%20no%20que%20el%20proceso%20siga%20vivo.md)
- [Las tareas periódicas salen de la aplicación a un disparador externo autenticado](notas/Las%20tareas%20peri%C3%B3dicas%20salen%20de%20la%20aplicaci%C3%B3n%20a%20un%20disparador%20externo%20autenticado.md)
- [Una copia de seguridad que no sale del servidor no es una copia de seguridad](notas/Una%20copia%20de%20seguridad%20que%20no%20sale%20del%20servidor%20no%20es%20una%20copia%20de%20seguridad.md)
- [Una tarea periódica debe correr como el usuario del servicio](notas/Una%20tarea%20peri%C3%B3dica%20debe%20correr%20como%20el%20usuario%20del%20servicio.md)
- [Git Bash reescribe las rutas absolutas que se pasan a un contenedor](notas/Git%20Bash%20reescribe%20las%20rutas%20absolutas%20que%20se%20pasan%20a%20un%20contenedor.md)
- [Un volcado de MariaDB 10.11 no se importa tal cual en MariaDB 10.4](notas/Un%20volcado%20de%20MariaDB%2010.11%20no%20se%20importa%20tal%20cual%20en%20MariaDB%2010.4.md)
### [Orquestación de flujos](notas/Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)

Flujos de automatización (n8n) con varias fuentes, cuotas, ramas y pruebas.

- [Una rama sin resultados no es un error del flujo](notas/Una%20rama%20sin%20resultados%20no%20es%20un%20error%20del%20flujo.md)
- [Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar](notas/Una%20fuente%20externa%20con%20cuota%20debe%20fallar%20hacia%20continuar%2C%20no%20hacia%20abortar.md)
- [Un nodo Merge espera a todas sus entradas y una rama vacía lo deja colgado](notas/Un%20nodo%20Merge%20espera%20a%20todas%20sus%20entradas%20y%20una%20rama%20vac%C3%ADa%20lo%20deja%20colgado.md)
- [Varias fuentes heterogéneas se normalizan a una estructura interna antes de unirlas](notas/Varias%20fuentes%20heterog%C3%A9neas%20se%20normalizan%20a%20una%20estructura%20interna%20antes%20de%20unirlas.md)
- [Un flujo programado necesita también un disparador manual para poder probarlo](notas/Un%20flujo%20programado%20necesita%20tambi%C3%A9n%20un%20disparador%20manual%20para%20poder%20probarlo.md)
- [Ejecutar un flujo de n8n por línea de órdenes no tiene todo lo que tiene el editor](notas/Ejecutar%20un%20flujo%20de%20n8n%20por%20l%C3%ADnea%20de%20%C3%B3rdenes%20no%20tiene%20todo%20lo%20que%20tiene%20el%20editor.md)
- [Las APIs oficiales de las redes sociales no exponen los verbos de interacción](notas/Las%20APIs%20oficiales%20de%20las%20redes%20sociales%20no%20exponen%20los%20verbos%20de%20interacci%C3%B3n.md)

## Cómo se escriben

Las notas salen de un proceso diario que revisa lo trabajado y destila solo la técnica reutilizable, con una regla fija:
nunca cruzan nombres de empresa o clientes, servidores, rutas, credenciales ni personas. Un script valida el formato,
los enlaces y que ninguna nota quede sin enlazar desde su mapa.

## Licencia

Texto bajo [CC BY 4.0](LICENSE) © 2026 Carlos Robles Borrego. Puedes reutilizarlo citando la fuente.
