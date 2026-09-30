---
type: note
aliases:
  - Fallar hacia continuar
  - Política de error por nodo
  - Redundancia decorativa
tags:
  - orquestacion
  - automatizacion
  - integracion
---

# Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar

Añadir una segunda fuente de datos como respaldo de la primera es media tarea. La otra media —la que se olvida— es **cambiar la política de error de la primera**. Si el nodo que consulta la fuente principal conserva el comportamiento por defecto, que es abortar la ejecución al recibir un error, el respaldo **no llega a ejecutarse nunca**. La redundancia existe en el diagrama y no en la realidad.

El caso concreto que lo hace pasar desapercibido es el **agotamiento de cuota**. Un servicio de pago sin créditos no da un fallo de red ni un tiempo de espera: responde con un `400`, un `401` o un `429` perfectamente formados. Para el orquestador es una respuesta de error como cualquier otra y dispara el aborto. Para quien mira el historial, es un error legítimo que parece explicar la parada, y la pregunta "¿por qué no ha entrado la fuente alternativa?" tarda en formularse.

La corrección tiene dos piezas y hacen falta las dos:

1. **En el nodo**: política de error *continuar con salida normal* y emisión garantizada de datos, para que el flujo siga y el nodo siguiente se ejecute.
2. **En el normalizador de después**: tratar un cuerpo de error como *cero resultados*. Al continuar en vez de abortar, lo que baja por la rama ya no es la respuesta esperada sino el objeto de error del proveedor. Conviene comprobar explícitamente el campo de estado de la respuesta en lugar de dar por hecho que existe la lista de resultados.

Sin la segunda pieza, el error simplemente se traslada un nodo más abajo y revienta al intentar recorrer algo que no es una lista.

Regla de diseño: **un respaldo que no se ha probado rompiendo la fuente principal es una suposición, no un respaldo**. La prueba barata es invalidar temporalmente la credencial primaria y comprobar que la ejecución termina bien y con datos.

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Una rama sin resultados no es un error del flujo](Una%20rama%20sin%20resultados%20no%20es%20un%20error%20del%20flujo.md)
- [Varias fuentes heterogéneas se normalizan a una estructura interna antes de unirlas](Varias%20fuentes%20heterog%C3%A9neas%20se%20normalizan%20a%20una%20estructura%20interna%20antes%20de%20unirlas.md)
