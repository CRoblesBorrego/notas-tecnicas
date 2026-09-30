---
type: note
aliases:
  - Middleware y rutas de API
  - Proxy de Next.js
tags:
  - desarrollo
  - nextjs
  - autenticacion
---

# El middleware no puede redirigir al login las rutas que consume un cliente no navegador

El guardián de sesión más común en una aplicación web es una capa que se ejecuta antes de cada ruta: si no hay cookie de sesión, `302` a `/login`. Para un navegador es exactamente lo que se quiere. Para cualquier otro cliente es una trampa.

Un `curl`, una descarga de CSV, una tarea programada que llama a un endpoint o un monitor externo **esperan un código de estado**, no una redirección a HTML. Al recibir el `302` siguen el salto, obtienen un `200` con la página de login y lo dan por bueno. El fallo es silencioso en los dos sentidos: el cron cree que ejecutó la tarea, y el fichero descargado contiene el formulario de acceso en vez de los datos.

La solución es una lista de rutas que **se autentican ellas mismas** —devolviendo `401` desde su propio manejador— y que la capa de guardia deja pasar sin tocar. Dos detalles que deciden si eso es seguro:

- **La comparación debe ser por igualdad exacta, no por prefijo.** Si la excepción se evalúa con `startsWith`, una ruta `/api/health` abre también `/api/health-de-mentira` y cualquier cosa que cuelgue debajo. El prefijo es cómodo para la lista de rutas *públicas*; para la lista de excepciones es una vulnerabilidad de autorización.
- **Dejar pasar no es autorizar.** Cada ruta de la lista tiene que comprobar la sesión o el secreto por su cuenta, en su propio código. Si no, la excepción es simplemente un agujero.

Detalle de versión: en **Next.js 16 el fichero `middleware.ts` quedó obsoleto y pasó a llamarse `proxy.ts`**, con la función exportada renombrada de `middleware` a `proxy`. Hay un *codemod* oficial para migrarlo. El motivo declarado del cambio es que "middleware" se confundía con el de Express y animaba a usarlo demasiado; la recomendación explícita del equipo es tratarlo como último recurso. Nota relacionada: la documentación advierte de que las *Server Functions* viajan como `POST` a la ruta donde se usan, así que un `matcher` que excluya un camino **también deja fuera sus Server Functions** — la autorización se comprueba dentro de cada una, nunca solo en esta capa.

## Las subidas grandes se cortan si el proxy las ve

Hay una segunda razón para que las rutas de API no pasen por el proxy aunque este las deje seguir sin hacer nada: **Next.js 16 copia en memoria el cuerpo de toda petición que el `matcher` incluye**, y el límite por defecto de esa copia es **10 MB** (`proxyClientMaxBodySize`, antes `middlewareClientMaxBodySize`). Lo que pasa de ahí no da error: se escribe un aviso en consola y **al manejador le llega el cuerpo truncado**. Una subida de 15 MB a una ruta que admite 25 acaba en un `formData()` que revienta con un `500`, o peor, en un archivo corrupto guardado como bueno. Las pruebas con archivos pequeños no lo detectan nunca.

La salida limpia es excluir `api` del `matcher` (`/((?!api|_next|favicon.ico|.*\\..*).*)`) —cada ruta se autentica sola, como se dice arriba— y comprobar `Content-Length` en la propia ruta antes de leer el cuerpo, para rechazar con `413` lo que exceda el máximo real.

## Fuentes

- [proxy.js — Next.js Docs](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
- Código de Next.js 16.3 (`server/body-streams.js`, `server/config-shared.js`): límite por defecto de 10 485 760 bytes y truncado con aviso, comprobado en 2026-09.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
- [Las tareas periódicas salen de la aplicación a un disparador externo autenticado](Las%20tareas%20peri%C3%B3dicas%20salen%20de%20la%20aplicaci%C3%B3n%20a%20un%20disparador%20externo%20autenticado.md)
