---
type: note
aliases:
  - defaultValue y router.refresh
  - Reiniciar un componente con key
tags:
  - desarrollo
  - react
  - nextjs
---

# Un select no controlado no se entera de que los datos del servidor cambiaron

En React, un campo **no controlado** (`<select defaultValue={estado}>`, `<input defaultValue=…>`) solo usa su valor inicial al montarse; después el valor vive en el DOM. Si la página vuelve a pedir los datos al servidor sin desmontar el componente —un `router.refresh()` en Next.js, una revalidación, una respuesta que cambia el estado como efecto secundario—, la propiedad llega nueva pero **el campo sigue mostrando el valor viejo**.

El caso que lo destapa es un efecto lateral: se responde a una petición, el servidor la pasa de «Abierta» a «Respondida» y refresca la página; el hilo muestra la respuesta, pero el selector de estado sigue en «Abierta». Nada falla, nada avisa; el dato correcto está en la base de datos y solo aparece al recargar. Peor aún: si el usuario toca el selector creyendo lo que ve, puede devolver la petición a un estado que ya no era el real.

Dos arreglos, según cuánto control haga falta:

- **Reiniciar el componente con `key`.** `<Selector key={estado} estado={estado} />`: cuando cambia el valor que viene del servidor, React desmonta y vuelve a montar, y `defaultValue` se aplica de nuevo. Es un cambio de una línea y conserva la sencillez del campo no controlado.
- **Pasarlo a controlado** (`value` + `onChange` con estado propio sincronizado con la propiedad). Más código, necesario si el componente tiene más estado interno que perder.

La regla general: todo campo no controlado que pinte un dato que el servidor puede cambiar por su cuenta necesita una `key` derivada de ese dato.

## Fuentes

- [Resetting state with a key — React](https://react.dev/learn/preserving-and-resetting-state#option-2-resetting-state-with-a-key)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Un control que depende del cliente debe reservar su hueco hasta montarse](Un%20control%20que%20depende%20del%20cliente%20debe%20reservar%20su%20hueco%20hasta%20montarse.md)
