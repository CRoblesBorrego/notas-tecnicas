---
type: note
aliases:
  - Disparador manual junto al programado
  - Probar un flujo sin esperar al horario
tags:
  - orquestacion
  - automatizacion
  - n8n
---

# Un flujo programado necesita también un disparador manual para poder probarlo

Un flujo cuyo único punto de entrada es un disparador de horario **no se puede lanzar bajo demanda**. Quedan dos opciones, y las dos son malas: esperar al siguiente ciclo —que en un flujo diario significa un intento de depuración por día— o abrir la interfaz gráfica y pulsar el botón, cosa que no siempre está disponible cuando el servicio corre en un servidor sin escritorio y solo se llega por terminal.

La restricción es concreta en n8n: la orden `n8n execute` arranca un flujo a través de un nodo **Execute Workflow Trigger**. Si el flujo no tiene ninguno, la línea de órdenes no puede iniciarlo por mucho que exista el disparador de horario. La solución es añadir un segundo nodo disparador y **conectarlo a los mismos destinos** que el programado, copiando su lista de conexiones en lugar de escribirla a mano: así los dos caminos ejecutan exactamente lo mismo y no se desincronizan cuando alguien reordene el flujo.

El nodo añadido es inerte: no se activa solo, no altera el horario y no consume nada mientras nadie lo llame. El coste de tenerlo es cero y lo que compra es la posibilidad de **probar en el sitio real sin esperar y sin tocar la programación**.

La regla general va más allá de una herramienta: **toda automatización que corre por temporizador necesita una entrada manual equivalente**. Sin ella, el ciclo de prueba dura lo que dure el intervalo, la depuración acaba haciéndose en producción a base de esperar, y la tentación de "adelantar el cron un momento para probar" introduce ejecuciones fuera de hora que luego nadie recuerda haber provocado. La entrada manual debe recorrer **el mismo camino** que la automática; si es un atajo que salta pasos, prueba otra cosa distinta de la que se quería probar.

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Las tareas periódicas salen de la aplicación a un disparador externo autenticado](Las%20tareas%20peri%C3%B3dicas%20salen%20de%20la%20aplicaci%C3%B3n%20a%20un%20disparador%20externo%20autenticado.md)
