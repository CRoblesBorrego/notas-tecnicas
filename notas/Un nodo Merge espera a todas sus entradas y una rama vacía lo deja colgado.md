---
type: note
aliases:
  - Merge frente a concatenación
  - Fan-in de ramas opcionales
tags:
  - orquestacion
  - automatizacion
  - n8n
---

# Un nodo Merge espera a todas sus entradas y una rama vacía lo deja colgado

Al unir varias ramas de un flujo, el nodo que parece obvio es el **Merge**. Es la elección equivocada cuando las ramas son **fuentes opcionales e intercambiables**, porque un Merge tiene semántica de *espera*: se queda aguardando a que todas sus entradas traigan datos. Si una fuente está caída, sin cuota o simplemente no ha encontrado nada, la unión no se resuelve y el flujo se queda parado detrás de ella.

La alternativa es no usar nodo de unión: **conectar las salidas de todas las ramas a la misma entrada de un nodo de código**. Varias conexiones sobre una única entrada se **concatenan** — el nodo recibe los items de todas ellas juntos, y las ramas que no aportaron nada simplemente no aportan items. Ese nodo hace de punto de unión y, de paso, es el sitio natural para filtrar lo que no cumple el contrato interno.

El criterio para elegir entre los dos:

- **Merge** cuando hay que *emparejar* datos relacionados: combinar por clave, cruzar el resultado de dos consultas que describen la misma entidad, esperar deliberadamente a que ambas partes estén listas. Ahí la espera es la función, no el estorbo.
- **Concatenación en una entrada** cuando es una *unión* de orígenes equivalentes: cinco fuentes que producen lo mismo y de las que basta con que responda alguna.

Lo que se pierde al renunciar al Merge son sus modos de combinación por campo, que en una unión de fuentes intercambiables no se necesitan: los items ya vienen normalizados a la misma forma y solo hay que apilarlos.

El patrón se generaliza más allá de una herramienta concreta: en cualquier orquestador, **un punto de sincronización convierte la fuente más lenta o más frágil en el límite de todo el flujo**. Si las ramas son redundantes, hay que unirlas sin sincronizar.

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Varias fuentes heterogéneas se normalizan a una estructura interna antes de unirlas](Varias%20fuentes%20heterog%C3%A9neas%20se%20normalizan%20a%20una%20estructura%20interna%20antes%20de%20unirlas.md)
- [Una rama sin resultados no es un error del flujo](Una%20rama%20sin%20resultados%20no%20es%20un%20error%20del%20flujo.md)
