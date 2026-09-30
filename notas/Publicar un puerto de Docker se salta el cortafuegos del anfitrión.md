---
type: note
aliases:
  - Docker y ufw
  - Puertos publicados
tags:
  - despliegue
  - docker
  - ciberseguridad
  - redes
---

# Publicar un puerto de Docker se salta el cortafuegos del anfitrión

Es una de las exposiciones accidentales más comunes en servidores propios, y su gravedad está en que **el administrador cree tener el puerto cerrado**.

El mecanismo: Docker gestiona el reenvío de puertos escribiendo sus propias reglas en la tabla `nat` de iptables. Los paquetes destinados a un contenedor **se desvían antes de llegar a las cadenas `INPUT` y `OUTPUT`**, que son justo donde `ufw` pone sus reglas. Resultado: `ufw deny 5432` se queda escrito, se ve en el estado del cortafuegos, y no bloquea absolutamente nada si ese puerto está publicado por un contenedor.

Y publicar es el comportamiento por defecto en cuanto se escribe la forma corta:

```yaml
ports:
  - "5432:5432"      # equivale a 0.0.0.0:5432 → todo internet
  - "127.0.0.1:5432:5432"   # solo desde el propio servidor
```

Reglas de higiene que salen de ahí:

- **Nada se publica si no tiene que atenderse desde fuera.** Los servicios que solo hablan entre sí —base de datos, caché, colas— se comunican por la red interna del *compose* usando el nombre del servicio como host, y no llevan `ports` en absoluto.
- Si hace falta publicar para trabajar desde el propio servidor (un cliente de base de datos, un volcado), se **ata al bucle local** y se llega por túnel SSH.
- La aplicación web tampoco necesita `0.0.0.0`: se ata al bucle local y delante va un **proxy inverso que termina TLS**. Así el certificado, las cabeceras de seguridad y la limitación de peticiones viven en un solo sitio, y la aplicación nunca ve tráfico crudo de internet.
- Lo que se filtra de verdad se filtra en la cadena `DOCKER-USER` —que sí se evalúa para el tráfico de contenedores— o en el grupo de seguridad del proveedor, un nivel por encima del anfitrión.

Es un buen ejemplo de por qué la configuración por defecto es el enemigo: nadie decidió exponer la base de datos, simplemente escribió dos números separados por dos puntos.

## Fuentes

- [Packet filtering and firewalls — Docker Docs](https://docs.docker.com/engine/network/packet-filtering-firewalls/)
- [Compose file: services (ports, healthcheck, depends_on) — Docker Docs](https://docs.docker.com/reference/compose-file/services/)

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- Zero Trust elimina la confianza basada en la ubicación de red
