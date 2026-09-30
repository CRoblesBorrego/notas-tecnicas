---
type: note
aliases:
  - Adelgazar wp-content uploads
  - Miniaturas huérfanas de WordPress
tags:
  - desarrollo
  - wordpress
  - rendimiento
---

# Al cambiar de tema de WordPress sobran casi todas las miniaturas de uploads

Cada tamaño registrado con `add_image_size()` genera un fichero más por cada imagen subida. Los temas comerciales grandes registran veinte o treinta tamaños, así que una web con unos pocos miles de fotos acumula más de cien mil miniaturas. Suelen pesar más que las imágenes originales. Al sustituir ese tema por uno propio que usa ocho tamaños, el resto se queda en disco para siempre, porque WordPress no borra nada al desactivar un tema.

Adelgazar `uploads` antes de migrar tiene tres pasos, y el tercero es el que se olvida:

1. **Averiguar qué se usa de verdad.** Los tamaños que pide el tema nuevo (`wp_get_attachment_image( $id, 'nombre' )`), los predeterminados del panel y los ficheros enlazados por URL directa en el contenido o en las plantillas. Con eso se genera la lista de ficheros que se conservan, sacada de los metadatos de cada adjunto.
2. **Copiar solo esa lista, y reducir los originales** a un máximo razonable (1920 px) con la misma ruta y nombre, para que la base de datos siga apuntando bien.
3. **Reescribir los metadatos** (`_wp_attachment_metadata`). Hay que quitar de `sizes` las miniaturas que ya no existen y actualizar `width` y `height` de los originales reducidos. Si no, `srcset` anuncia ficheros inexistentes y medidas falsas, y el navegador pide imágenes que devuelven `404`.

En un caso real, `uploads` pasó de 8,4 GB a 1,8 GB, sin cambios visibles. Aprovechar para no subir originales enormes tiene otra ventaja: el servidor de destino suele ser compartido, y sin reducirlos no habría cabido.

## Fuentes

- [add_image_size() — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/add_image_size/)
- [wp_update_attachment_metadata() — WordPress Developer Resources](https://developer.wordpress.org/reference/functions/wp_update_attachment_metadata/)

## Relacionado

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
