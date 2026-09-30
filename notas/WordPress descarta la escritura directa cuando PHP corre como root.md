---
type: note
aliases:
  - FS_METHOD y ftpsockets
  - Actualizar plugins por CLI
tags:
  - desarrollo
  - wordpress
  - despliegue
---

# WordPress descarta la escritura directa cuando PHP corre como root

Lanzar una actualización de plugins o temas desde un script PHP en el servidor —sin WP-CLI, cargando `wp-load.php` y llamando al actualizador— falla de una manera desconcertante: `bulk_upgrade()` devuelve un resultado vacío o `false`, **sin error, sin excepción y sin nada en el registro**. Los directorios son escribibles, una escritura de prueba con `file_put_contents()` funciona, y aun así no se actualiza nada.

La causa está en cómo WordPress decide si puede escribir por sí mismo. `get_filesystem_method()` no comprueba los permisos: crea un fichero temporal y **compara su propietario con el del directorio de destino**. Si coinciden, asume que el proceso web es el dueño real y devuelve `direct`; si no, concluye que hará falta FTP y devuelve `ftpsockets`. Como no hay credenciales de FTP configuradas, el actualizador se rinde en silencio.

Ejecutando como `root`, el fichero temporal es de `root` y el directorio de plugins es del usuario del servidor web. No coinciden, y **ser root no ayuda: lo estropea**. Es de los pocos casos donde tener más privilegios da peor resultado que tener los justos.

Hay dos salidas, y no son equivalentes:

- **Ejecutar como el usuario del servidor web** (`www-data` o el que sea). Es la buena: reproduce exactamente las condiciones del panel de administración, así que si funciona por aquí funcionará por allí.
- **Definir `FS_METHOD` como `direct` en la configuración.** Funciona, pero desactiva la comprobación para todo el sitio y para siempre, incluida la ruta por HTTP. Es tapar el síntoma.

La lección general: cuando una herramienta **infiere** una capacidad en vez de comprobarla directamente, la inferencia puede fallar en entornos que no existían cuando se escribió. Esa comprobación está pensada para hospedaje compartido de hace quince años, y en un contenedor se equivoca.

Y un aviso de método: un fallo que no produce error no es un fallo menor, es un fallo peor. El resultado vacío se lee como «no había nada que hacer», que es justo la conclusión contraria a la verdadera.

## Fuentes

- [`get_filesystem_method()` — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/get_filesystem_method/)

## Relacionado

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
