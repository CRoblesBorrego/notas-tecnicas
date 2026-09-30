---
type: note
aliases:
  - Limpiar WordPress hackeado
  - Sumas de verificación del núcleo de WordPress
tags:
  - desarrollo
  - seguridad
  - wordpress
---

# Un WordPress comprometido se reconstruye con código oficial en vez de limpiarse

Las puertas traseras de un WordPress atacado están hechas para no parecerlo. Algunos patrones habituales:

- **Plugins falsos con nombre genérico y marca de tiempo** (`analytics-<timestamp>`, `slider-<timestamp>`). A menudo son una copia íntegra de un plugin legítimo con un fichero extra, así que quien abre la carpeta ve código normal. No hace falta que estén activados: se invocan directamente por su URL.
- **Ficheros sueltos en carpetas del núcleo o de traducciones** (`wp-includes/bottom.<timestamp>.php`), a veces con un comentario HTML aleatorio antes de `<?php`.
- **Nombres que imitan otros**: `singIe.php` con I mayúscula, `4O4.php` con la letra O.
- **Ejecución sin rastro**: reciben código cifrado por formulario, cookie o parámetro, lo escriben en `/tmp` o `/dev/shm`, lo incluyen y lo borran.

La marca de tiempo del nombre y la fecha de modificación reconstruyen la cronología de la intrusión.

Buscar fichero a fichero no da garantías. Lo fiable es:

1. **Comparar el núcleo** con las sumas oficiales (`api.wordpress.org/core/checksums/1.0/?version=X&locale=Y`). Todo fichero que sobre o no coincida es sospechoso.
2. **Revisar la base de datos**: administradores añadidos, scripts inyectados en entradas u opciones, tareas programadas extrañas. Suele estar limpia, y entonces el contenido se conserva.
3. **Reconstruir**: núcleo, tema y plugins descargados de su origen oficial. De la copia solo se reutilizan los medios, comprobando que en `uploads` no queda PHP.
4. **Cerrar la puerta de entrada**: cambiar todas las contraseñas y las claves de `wp-config.php` (el atacante pudo leerlo), impedir la ejecución de PHP en `uploads` desde el servidor web y bloquear `xmlrpc.php`.

La causa de fondo casi siempre es la misma: plugins y temas sin actualizar durante años, sobre todo los de pago, que no se actualizan solos.

## Fuentes

- [WordPress.org Core Checksums API](https://api.wordpress.org/core/checksums/1.0/)
- [Hardening WordPress — WordPress Developer Resources](https://developer.wordpress.org/advanced-administration/security/hardening/)

## Relacionado

- [XML-RPC multiplica los intentos de contraseña dentro de una sola petición](XML-RPC%20multiplica%20los%20intentos%20de%20contrase%C3%B1a%20dentro%20de%20una%20sola%20petici%C3%B3n.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
