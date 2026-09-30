---
type: note
aliases:
  - 403 del CDN frente a 403 de la API
  - User-Agent por defecto
  - Error 1010 de Cloudflare
tags:
  - integracion
  - api
  - http
---

# Un 403 puede estar rechazando al cliente y no a la clave

Cuando una API devuelve `403`, el reflejo es sospechar de la credencial: caducada, con el nombre de cabecera equivocado, con un prefijo `Bearer` de más o de menos. A veces no es eso. Muchas APIs están detrás de una red de distribución de contenido que **filtra antes de que la petición llegue al servicio**, y ese filtro no mira la clave: mira la **firma del cliente**.

El agente de usuario por defecto de las bibliotecas HTTP —`Python-urllib/3.x` y equivalentes— aparece en las listas de bloqueo por su uso masivo en automatismos indiscriminados. El resultado es un `403` para un cliente perfectamente legítimo con una clave perfectamente válida. Cloudflare, por ejemplo, lo señala con el **código de error 1010** en el cuerpo de la respuesta.

El síntoma que lo delata: **la misma clave funciona desde `curl` o desde el navegador y falla desde el script**. Si la credencial fuera el problema, fallaría igual en los tres sitios.

De ahí un orden de diagnóstico que ahorra horas:

1. **Leer el cuerpo de la respuesta, no solo el código.** El `403` no distingue, el cuerpo sí: dice si el rechazo viene del filtro perimetral o del servicio. Es información que se pierde si el manejador de errores solo registra el número.
2. **Variar una cosa cada vez**: agente de usuario, nombre de la cabecera de autenticación, valor de la clave. Cambiar dos a la vez y ver que funciona no dice cuál era.
3. **Comprobar la credencial contra la API real**, no razonar sobre ella. Una consulta mínima con la clave guardada distingue en un segundo entre "no vale" y "no me dejan entrar".

Poner un agente de usuario descriptivo en cualquier cliente propio es buena práctica al margen de esto: identifica el tráfico en los registros del proveedor y evita quedar barrido por una regla genérica.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Una fuente externa con cuota debe fallar hacia continuar, no hacia abortar](Una%20fuente%20externa%20con%20cuota%20debe%20fallar%20hacia%20continuar%2C%20no%20hacia%20abortar.md)
