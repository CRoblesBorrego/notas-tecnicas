---
type: note
aliases:
  - Healthcheck
  - Sonda de salud
  - Liveness y readiness
tags:
  - despliegue
  - docker
  - observabilidad
---

# Una sonda de salud comprueba las dependencias, no que el proceso siga vivo

Un endpoint de salud que devuelve `200 OK` sin tocar nada solo demuestra que el proceso acepta conexiones. Eso es cierto también cuando la base de datos lleva veinte minutos caída y **todas** las peticiones reales fallan. La sonda útil hace la comprobación mínima que ejercita la dependencia crítica: una consulta trivial tipo `SELECT 1`.

Forma de la respuesta:

- **`200` con un cuerpo mínimo** cuando todo responde; **`503`** cuando la dependencia no contesta. El código de estado es lo que leen el orquestador y el proxy inverso: el cuerpo es para los humanos.
- **`Cache-Control: no-store`**. Una sonda cacheada informa del pasado, que es exactamente lo contrario de para lo que existe.
- **No expone nada**: ni versiones, ni nombres de host, ni cadenas de conexión, ni el mensaje de la excepción. Suele ser la única ruta sin autenticar de la aplicación, así que es también la más fácil de encontrar para un atacante; que diga "sin conexión" y no el error de la base de datos.

La distinción que casi siempre se pasa por alto es entre **estar vivo** y **estar listo**:

- *Vivo* responde a "¿hay que reiniciar este proceso?". Debe ser barato y no depender de terceros.
- *Listo* responde a "¿le mando tráfico?". Ahí sí entran las dependencias.

Mezclarlas tiene un modo de fallo desagradable: si la sonda que decide reinicios consulta la base de datos, un corte de treinta segundos en la base **reinicia todas las réplicas de la aplicación a la vez**, y cuando la base vuelve se encuentra una tormenta de arranques en frío. El corte se convierte en una caída más larga por culpa de la sonda.

En `docker compose` esto se traduce en dos ajustes que evitan falsos positivos: `start_period`, para no contar fallos durante el arranque —migraciones incluidas—, y `depends_on` con `condition: service_healthy`, que retrasa el arranque de la aplicación hasta que la base de datos acepta conexiones de verdad, no hasta que su contenedor existe.

## Fuentes

- [Compose file: services (healthcheck, depends_on) — Docker Docs](https://docs.docker.com/reference/compose-file/services/)

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- Sin telemetría no hay detección posible
