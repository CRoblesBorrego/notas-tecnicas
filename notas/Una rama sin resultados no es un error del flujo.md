---
type: note
aliases:
  - Ausencia de resultados no es error
  - Guarda que aborta
  - Centinela vacío
tags:
  - orquestacion
  - automatizacion
  - n8n
---

# Una rama sin resultados no es un error del flujo

En un flujo de automatización es tentador poner una guarda al principio: si la búsqueda no devuelve nada, `throw new Error('Ningún resultado')` y se acabó la ejecución. Con **una sola fuente** la guarda es razonable: sin datos no hay nada que hacer, y el error deja rastro en el historial de ejecuciones.

El problema aparece al añadir la segunda fuente. Esa misma guarda ya no aborta *su* rama: aborta **la ejecución entera**, incluidas las ramas hermanas que sí habían traído datos. Una fuente agotada tumba a las que funcionaban, y el resultado es un flujo que no publica nada durante semanas mientras el historial muestra un error que parece explicarlo todo — "no había resultados" — cuando en realidad sí los había en otra rama.

La corrección es distinguir dos situaciones que el código había mezclado:

- **No hay datos**: resultado válido y esperado. La rama emite un **item centinela vacío** (`return [{ json: {} }]`) y sigue adelante.
- **No he podido trabajar**: credencial inválida, respuesta ilegible, contrato roto. Eso sí es un error y debe verse.

El centinela no ensucia nada si el nodo de unión ya descarta los items que no traen los campos obligatorios, que es la disciplina sana de todas formas. Hace falta además marcar el nodo para que **siempre emita datos**: un nodo que no produce ninguna salida impide que se ejecute el siguiente, y por esa vía la rama vacía vuelve a arrastrar al resto.

La lección general: **una guarda escrita cuando había un solo camino codifica el supuesto de que sigue habiendo uno solo**. Al ramificar el flujo hay que revisarlas todas, porque ninguna avisa de que ha dejado de ser correcta. Es el mismo error que un `exit 1` en un script que pasa de hacer una cosa a hacer tres.

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar](Una%20fuente%20externa%20con%20cuota%20debe%20fallar%20hacia%20continuar%2C%20no%20hacia%20abortar.md)
- [Un nodo Merge espera a todas sus entradas y una rama vacía lo deja colgado](Un%20nodo%20Merge%20espera%20a%20todas%20sus%20entradas%20y%20una%20rama%20vac%C3%ADa%20lo%20deja%20colgado.md)
