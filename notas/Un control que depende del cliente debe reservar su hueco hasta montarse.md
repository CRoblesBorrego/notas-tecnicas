---
type: note
aliases:
  - Hidratación y tema
  - Conmutador de tema
tags:
  - desarrollo
  - react
  - nextjs
  - interfaz
---

# Un control que depende del cliente debe reservar su hueco hasta montarse

Cualquier control cuyo aspecto dependa de algo que **solo existe en el navegador** —el tema guardado en `localStorage`, la preferencia del sistema, el ancho de la ventana, la zona horaria del usuario— tiene un problema con el renderizado en servidor: el servidor no puede saber ese valor, así que pinta una cosa y el cliente pinta otra. React detecta la discrepancia al hidratar y avisa por consola, o directamente descarta el HTML del servidor.

El caso canónico es el conmutador de tema claro/oscuro: el servidor no sabe si toca sol o luna.

El patrón que lo resuelve tiene dos mitades:

1. **Un estado `mounted` que empieza en `false`** y pasa a `true` en un `useEffect`. Los efectos no se ejecutan en el servidor, así que el primer render del cliente coincide exactamente con el del servidor, y el icono real aparece en el segundo. La hidratación ya no tiene nada que reconciliar.
2. **Mientras tanto se devuelve un hueco de las mismas dimensiones**, no `null`. Devolver `null` es lo que sale solo y provoca un salto del diseño: el resto de la barra se recoloca cuando el botón aparece. Un `div` vacío con el mismo alto y ancho, marcado `aria-hidden`, mantiene el espacio reservado.

Detalles que suelen faltar en la implementación:

- **La preferencia del sistema no siempre debe heredarse.** Si el aspecto por defecto del producto es el claro, conviene desactivar la detección automática y arrancar en claro, dejando el oscuro como elección explícita del usuario. Es una decisión de producto, no un ajuste técnico: heredar el modo del sistema puede enseñar a la mitad de los usuarios una versión del producto que nadie ha revisado.
- **Desactivar las transiciones durante el cambio de tema.** Sin eso, las transiciones de color declaradas en CSS se disparan todas a la vez y el cambio se ve como un barrido sucio.
- Si el parpadeo inicial es inaceptable, la única forma real de evitarlo es un **script bloqueante en el `<head>`** que ponga la clase en el elemento raíz antes de pintar. El coste es una etiqueta de script en línea que hay que contemplar en la política de seguridad de contenido.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
