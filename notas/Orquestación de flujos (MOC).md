---
type: moc
title: Orquestación de flujos (MOC)
aliases:
  - MOC Orquestación
  - MOC Flujos
tags:
  - moc
  - orquestacion
  - automatizacion
---

# Orquestación de flujos — Mapa de contenido

Automatizaciones que encadenan fuentes externas, transformaciones y publicación en un orquestador visual del tipo n8n. Lo que reúne estas notas no es la herramienta, sino un modo de fallo propio de este tipo de sistemas: **el flujo se para entero y el historial de ejecuciones ofrece una explicación creíble y equivocada**. Nadie recibe una alerta, porque desde dentro parece que el trabajo se hizo y no había nada que hacer.

## Cuando una rama arrastra a todas

- [Una rama sin resultados no es un error del flujo](Una%20rama%20sin%20resultados%20no%20es%20un%20error%20del%20flujo.md) — distinguir *no hay datos* de *no he podido trabajar*; el item centinela y por qué una guarda escrita para un solo camino caduca al ramificar.
- [Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar](Una%20fuente%20externa%20con%20cuota%20debe%20fallar%20hacia%20continuar%2C%20no%20hacia%20abortar.md) — el respaldo que nunca se ejecuta porque la fuente principal aborta antes; la cuota agotada responde con un error bien formado.
- [Un nodo Merge espera a todas sus entradas y una rama vacía lo deja colgado](Un%20nodo%20Merge%20espera%20a%20todas%20sus%20entradas%20y%20una%20rama%20vac%C3%ADa%20lo%20deja%20colgado.md) — sincronizar frente a concatenar, y cuándo cada uno.

## Cómo se estructura un flujo con varias fuentes

- [Varias fuentes heterogéneas se normalizan a una estructura interna antes de unirlas](Varias%20fuentes%20heterog%C3%A9neas%20se%20normalizan%20a%20una%20estructura%20interna%20antes%20de%20unirlas.md) — un adaptador por proveedor, un contrato interno propio, y la procedencia que se registra pero no se consulta.
- [Un flujo programado necesita también un disparador manual para poder probarlo](Un%20flujo%20programado%20necesita%20tambi%C3%A9n%20un%20disparador%20manual%20para%20poder%20probarlo.md) — sin entrada manual, el ciclo de prueba dura lo que dure el intervalo y la depuración acaba en producción.
- [Ejecutar un flujo de n8n por línea de órdenes no tiene todo lo que tiene el editor](Ejecutar%20un%20flujo%20de%20n8n%20por%20l%C3%ADnea%20de%20%C3%B3rdenes%20no%20tiene%20todo%20lo%20que%20tiene%20el%20editor.md) — sin *Data Tables* en `n8n execute`; copia temporal con escrituras desactivadas y ejecución desde el editor.

## El límite lo pone la API, no el diseño

- [Las APIs oficiales de las redes sociales no exponen los verbos de interacción](Las%20APIs%20oficiales%20de%20las%20redes%20sociales%20no%20exponen%20los%20verbos%20de%20interacci%C3%B3n.md) — publicar y leer sí; dar me gusta y seguir no existen en la Graph API y en X se han ido cerrando. Hay que separar por escrito lo que la API permite de lo que el producto imagina.

## Hilo conductor

Las cinco comparten una causa: **el comportamiento por defecto del orquestador supone que el flujo es una línea recta**. Abortar al primer error, esperar a todas las entradas, arrancar solo por horario — todo eso es sensato mientras hay un camino único, y deja de serlo en cuanto se añade el segundo. Ninguna de esas decisiones se toma explícitamente; se heredan del flujo original y sobreviven a la refactorización que las invalida.

De ahí dos hábitos que valen más que cualquier truco concreto: **al ramificar un flujo, releer todas las guardas y políticas de error que se escribieron cuando era lineal**, y **probar la redundancia rompiendo a propósito la fuente principal**, que es la única forma de saber si el respaldo existe de verdad.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md) — el otro extremo de la tubería: publicar por API en un gestor de contenidos.
- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md) — dónde corre todo esto.
- [Índice de Notas](../README.md)
