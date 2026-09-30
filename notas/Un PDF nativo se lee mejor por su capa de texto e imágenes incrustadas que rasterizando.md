---
type: note
aliases:
  - Extraer contenido de un PDF
  - pdf-parse
  - Imágenes incrustadas de un PDF
tags:
  - desarrollo
  - datos
  - automatizacion
---

# Un PDF nativo se lee mejor por su capa de texto e imágenes incrustadas que rasterizando

Cuando hay que pasar un PDF a otro formato —una página web, un gestor de contenidos, un índice de búsqueda—, el reflejo es rasterizar: convertir cada página a imagen y, si hace falta, reconocer el texto. Es lo que hacen muchas herramientas por defecto y lo que obliga a instalar dependencias del sistema como `poppler`.

Para un PDF **nativo** (generado por software, no escaneado) es la vía peor. Ese fichero ya contiene dos cosas separadas y limpias: una **capa de texto** con los caracteres reales y un conjunto de **imágenes incrustadas** en su resolución original. Extraer ambas por separado da texto sin errores de reconocimiento e imágenes mejores que cualquier captura de pantalla de la página, y no necesita binarios externos: basta una biblioteca del propio lenguaje.

Cuatro cosas que hay que saber al hacerlo:

- **Las imágenes vienen agrupadas por página y sin nombre útil.** El nombre que devuelve la biblioteca se repite entre páginas; guardarlas con ese nombre significa **sobrescribir en silencio** y quedarse solo con la última de cada página. Hay que componer el nombre con el número de página y el índice dentro de ella.
- **Sale también el mobiliario del documento.** Cabeceras, logotipos, iconos decorativos: aparecen una vez por página y ensucian el resultado. Se filtran bien por dimensiones y por repetición — un mismo tamaño exacto en todas las páginas casi siempre es plantilla, no contenido.
- **El formato real se detecta por los primeros bytes**, no por la extensión que sugiera la biblioteca. La cabecera distingue PNG, JPEG, GIF y WebP en dos bytes.
- **El recuento de páginas puede no cuadrar** con el que informan otras herramientas, que a veces cuentan objetos renderizables en lugar de páginas. Conviene comprobarlo antes de asumir que la extracción se quedó corta.

Sobre el orden: el texto extraído conserva el **orden del flujo de contenido**, que no siempre coincide con el orden visual. Los pies de figura pueden salir antes que la imagen a la que acompañan. Para reconstruir la maquetación hay que apoyarse en el número de página y en la numeración explícita de las figuras, no en la posición dentro del texto plano.

Nota de versión: en `pdf-parse` la API cambió entre la versión 1 y la 2. La 1 exportaba una función por defecto; la 2 exporta una clase `PDFParse` con métodos `getText()`, `getImage()` y `getInfo()`. El código antiguo falla con un escueto *"is not a function"*, que no delata la causa.

## Fuentes

- [`pdf-parse` — npm](https://www.npmjs.com/package/pdf-parse)

## Relacionado

- [Cargar wp-load.php da acceso completo a WordPress cuando no hay WP-CLI](Cargar%20wp-load.php%20da%20acceso%20completo%20a%20WordPress%20cuando%20no%20hay%20WP-CLI.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
