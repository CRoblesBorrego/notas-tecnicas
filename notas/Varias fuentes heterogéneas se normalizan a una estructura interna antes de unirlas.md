---
type: note
aliases:
  - Normalizador por fuente
  - Adaptador de fuentes
  - Estructura interna común
tags:
  - orquestacion
  - automatizacion
  - integracion
---

# Varias fuentes heterogéneas se normalizan a una estructura interna antes de unirlas

Cuando un flujo consulta varios proveedores que devuelven lo mismo con nombres distintos —uno llama `date` a la fecha y `source` al medio, otro `pubDate` y `source_id`—, hay dos sitios donde absorber la diferencia: **en cada rama, antes de unir**, o **repartida por todo lo que viene después**. Solo el primero escala.

El patrón es un **adaptador por fuente**: cada rama termina en un nodo que traduce la respuesta del proveedor a una estructura interna fija (`titulo`, `link`, `fecha`, `fuente`…). A partir del punto de unión, ningún nodo sabe ni necesita saber de dónde vino cada item. Añadir un quinto proveedor es entonces **un nodo de consulta y un normalizador, con cero cambios aguas abajo** — y quitarlo es borrar dos nodos. La alternativa, dejar que la forma cruda del proveedor se propague, obliga a que cada filtro, cada plantilla y cada condicional posterior conozca todos los formatos, y convierte cada fuente nueva en una modificación de todo el flujo.

El normalizador es además el sitio donde concentrar tres cosas que si no se dispersan:

- **Descartar lo inservible**: items sin los campos obligatorios, que ninguna etapa posterior podrá usar.
- **Absorber el cuerpo de error** del proveedor cuando la rama tolera fallos, traduciéndolo a cero resultados en vez de dejar que reviente más abajo.
- **Sellar la procedencia** en un campo (`origen`). Sirve para depurar y para medir qué fuente aporta qué, sin que nadie ramifique según su valor.

Ese último punto marca la frontera del patrón: la procedencia se **registra**, no se **consulta**. En cuanto un nodo posterior empieza a preguntar de qué proveedor viene un item para tratarlo distinto, la normalización ha dejado de ser completa y conviene mover esa diferencia al adaptador correspondiente.

Es el patrón adaptador clásico, aplicado a un orquestador visual en vez de a un lenguaje de programación: la lógica común se escribe una vez contra un contrato propio, y cada integración externa paga su propia traducción.

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Un nodo Merge espera a todas sus entradas y una rama vacía lo deja colgado](Un%20nodo%20Merge%20espera%20a%20todas%20sus%20entradas%20y%20una%20rama%20vac%C3%ADa%20lo%20deja%20colgado.md)
- [Una integración se ata al identificador estable, no al nombre visible](Una%20integraci%C3%B3n%20se%20ata%20al%20identificador%20estable%2C%20no%20al%20nombre%20visible.md)
