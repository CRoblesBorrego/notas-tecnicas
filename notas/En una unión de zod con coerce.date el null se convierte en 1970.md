---
type: note
aliases:
  - zod coerce y null
  - Orden de las uniones en zod
tags:
  - desarrollo
  - typescript
  - validacion
---

# En una unión de zod con coerce.date el null se convierte en 1970

Para un campo de fecha que se puede borrar, lo natural es aceptar «una fecha o `null`»: `z.union([z.coerce.date(), z.null()])`. Parece correcto y pasa cualquier prueba que envíe fechas. Pero **zod prueba las opciones de una unión en orden y se queda con la primera que acepta el valor**, y `z.coerce.date()` acepta `null`: lo pasa por `new Date(null)`, que es una fecha válida, **el 1 de enero de 1970**. La rama `z.null()` nunca llega a ejecutarse.

El resultado es un fallo silencioso: la API responde `200`, el cliente cree que ha vaciado la fecha, y en la base de datos queda guardada la época Unix. Lo mismo pasa con cualquier número (`0`, un `true` convertido): todo lo que `new Date()` acepte se cuela como fecha. La cadena vacía, en cambio, da `Invalid Date` y sí se rechaza.

Reglas que lo evitan:

- **En una unión, la opción más estricta va primero**: `z.union([z.null(), z.coerce.date()])`, o mejor `z.coerce.date().nullable()`, que comprueba `null` antes de convertir.
- **No usar `coerce` sobre entrada arbitraria** cuando el formato es conocido. Para un día de calendario es más seguro validar la cadena `YYYY-MM-DD` con una expresión regular y comprobar que existe (`2026-02-30` no existe), en vez de convertir y aceptar lo que salga.
- Probar explícitamente el valor «borrar» (`null`, `""`), no solo los valores buenos.

## Fuentes

- [Unions — Zod docs](https://zod.dev/api#unions) (las opciones se evalúan en orden y gana la primera válida)
- [Coercion — Zod docs](https://zod.dev/api#coercion)

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
