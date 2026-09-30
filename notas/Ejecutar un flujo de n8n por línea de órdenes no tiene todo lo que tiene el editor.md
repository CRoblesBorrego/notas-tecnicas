---
type: note
aliases:
  - n8n execute y Data Tables
  - Probar flujos de n8n sin el editor
tags:
  - n8n
  - automatizacion
  - pruebas
---

# Ejecutar un flujo de n8n por línea de órdenes no tiene todo lo que tiene el editor

n8n permite lanzar un flujo desde la terminal (`n8n execute --id=<flujo>`), lo que tienta para automatizar pruebas sin abrir el navegador. Pero ese proceso arranca una instancia aparte con menos módulos que el servidor: en n8n 2.x, **los nodos de *Data Tables* fallan con «the module is disabled»**, aunque en el editor el mismo flujo funcione. Además, si el servidor está en marcha, la instancia de la terminal choca con sus puertos (el del *broker* de ejecutores y el HTTP) y hay que moverla con variables de entorno solo para que arranque.

Consecuencias prácticas:

- Un flujo que guarda o consulta estado en tablas propias **solo se puede probar de punta a punta desde el editor** (botón de ejecutar, con un disparador manual) o activándolo y provocando su disparador real.
- Para probar sin ensuciar los datos reales, se importa una **copia temporal** del flujo con los nodos de escritura desactivados (`disabled: true`) y la configuración de un cliente ficticio, se ejecuta desde el editor y se borra al terminar. En n8n 2.x borrar exige archivarlo antes.
- La lógica de los nodos de código se prueba aparte, fuera de n8n, ejecutando los mismos archivos con datos de ejemplo; así el editor solo hace falta para la integración.

## Fuentes

- Comprobado con n8n 2.40 en 2026-09 (`n8n execute` frente a ejecución desde el editor).
- [CLI commands — n8n Docs](https://docs.n8n.io/hosting/cli-commands/)

## Relacionado

- [Orquestación de flujos (MOC)](Orquestaci%C3%B3n%20de%20flujos%20%28MOC%29.md)
- [Un flujo programado necesita también un disparador manual para poder probarlo](Un%20flujo%20programado%20necesita%20tambi%C3%A9n%20un%20disparador%20manual%20para%20poder%20probarlo.md)
