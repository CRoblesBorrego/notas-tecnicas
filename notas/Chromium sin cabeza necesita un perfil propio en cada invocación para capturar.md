---
type: note
aliases:
  - headless screenshot
  - user-data-dir
  - Capturas por lotes con Chromium
tags:
  - desarrollo
  - automatizacion
  - navegador
---

# Chromium sin cabeza necesita un perfil propio en cada invocación para capturar

`chrome --headless=new --screenshot=salida.png fichero.html` es la forma más barata de convertir una página en imagen: no hace falta instalar ningún controlador de automatización. Funciona la primera vez y falla a partir de la segunda de una manera especialmente molesta — **devuelve código de salida 0 y no escribe el fichero**.

La causa es el **perfil de usuario**. Chromium bloquea el directorio de perfil mientras lo usa, y no libera el bloqueo inmediatamente al terminar. Una segunda invocación que apunte al mismo directorio encuentra el candado, decide no arrancar y sale limpiamente, como si hubiera hecho el trabajo. Es peor que un error: un bucle que genera veinte capturas produce una y notifica éxito veinte veces.

Con esto en mente, el patrón para lotes es:

- **Un `--user-data-dir` distinto por invocación**, no uno compartido. El coste en disco es despreciable frente al de depurar el silencio.
- **Comprobar que el fichero existe** después de cada llamada, en lugar de fiarse del código de salida. Es la única señal fiable.
- **`--virtual-time-budget`** para dar tiempo a que carguen tipografías y recursos remotos: sin él la captura sale con la tipografía de reserva.
- **`--window-size`** define el alto capturado, porque la captura es del *viewport*, no de la página entera. Alturas muy grandes fallan; para una página larga salen más a cuenta varias capturas desplazando el contenido con un margen superior negativo que una sola gigante.

La lección transferible no es sobre navegadores: **una herramienta que se serializa mediante un bloqueo de fichero acaba, tarde o temprano, tratando "no pude ejecutarme" como "no hay nada que hacer"**. Cada vez que un proceso por lotes reutiliza un recurso exclusivo entre iteraciones, la verificación no puede ser el código de salida; tiene que ser el efecto observable.

## Fuentes

- [Chrome headless mode — Chrome for Developers](https://developer.chrome.com/docs/chromium/headless)

## Relacionado

- [Un PDF nativo se lee mejor por su capa de texto e imágenes incrustadas que rasterizando](Un%20PDF%20nativo%20se%20lee%20mejor%20por%20su%20capa%20de%20texto%20e%20im%C3%A1genes%20incrustadas%20que%20rasterizando.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
