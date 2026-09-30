---
type: note
aliases:
  - Build hermético
  - Collecting page data
tags:
  - desarrollo
  - despliegue
  - docker
  - nextjs
---

# Construir la imagen de una app no debe requerir la base de datos

Un `docker build` que falla porque no encuentra la base de datos casi nunca es un problema de red: es que **algo se ejecuta al importar un módulo** y nadie lo había notado, porque en desarrollo la base siempre estaba ahí.

El caso típico en Next.js con App Router: el paso *Collecting page data* del `next build` **importa todos los módulos de ruta** para leer su configuración (`dynamic`, `revalidate`, `runtime`), incluidos los manejadores de API y las rutas ya marcadas como dinámicas. Si el cliente de base de datos se construye en el ámbito del módulo —`export const db = new Client(process.env.DATABASE_URL)`— esa construcción ocurre **durante el build**, sin cadena de conexión, y revienta antes de renderizar nada.

Dos arreglos, con compromisos distintos:

- **Construcción perezosa**: envolver el cliente en un *getter* que lo crea en la primera consulta. Es la solución limpia, pero toca código de la aplicación y hay que sostener la disciplina.
- **Valor de relleno en el build**: inyectar una cadena de conexión falsa como variable de entorno en la etapa de construcción. Funciona porque los *pools* de conexión (`pg`, Prisma sobre adaptador) **conectan de forma perezosa en la primera consulta**: construir el cliente no abre ningún socket. Es una línea en el `Dockerfile`, pero deja el acoplamiento oculto en vez de resuelto.

El principio que hay detrás vale para cualquier lenguaje: **el build tiene que ser hermético**, es decir, reproducible sin secretos, sin red y sin servicios levantados. Si la construcción necesita un valor real, ese valor acaba fijado en una capa de la imagen —donde cualquiera con acceso al registro lo lee— o convierte la canalización de integración continua en un consumidor de credenciales de producción. El valor real se inyecta **en tiempo de ejecución**, no en tiempo de compilación.

Corolario práctico: si el `Dockerfile` necesita un comentario largo explicando por qué hay un valor de relleno, ese comentario es deuda documentada. Vale como parche, pero apunta a que la dependencia debería ser perezosa.

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- [Publicar un puerto de Docker se salta el cortafuegos del anfitrión](Publicar%20un%20puerto%20de%20Docker%20se%20salta%20el%20cortafuegos%20del%20anfitri%C3%B3n.md)
