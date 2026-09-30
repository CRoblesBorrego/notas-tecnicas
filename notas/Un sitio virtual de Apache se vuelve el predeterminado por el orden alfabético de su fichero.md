---
type: note
aliases:
  - default server de Apache
  - Orden de carga de sites-enabled
tags:
  - desarrollo
  - servidores
  - apache
---

# Un sitio virtual de Apache se vuelve el predeterminado por el orden alfabético de su fichero

En un Apache que aloja varias webs, cada puerto tiene un **sitio por defecto**: el que responde a cualquier petición cuyo `Host` no coincide con ningún `ServerName` ni `ServerAlias`. Nadie lo elige de forma explícita. Es simplemente **el primer `<VirtualHost>` que Apache lee para ese puerto**, y en Debian y derivados los ficheros de `sites-enabled` se cargan por orden alfabético.

La consecuencia es fácil de pasar por alto: dar de alta un sitio cuyo fichero se llame `a…` en un servidor donde hasta entonces mandaba `b…` cambia el comportamiento de **todas** las peticiones sin dueño. Si el sitio nuevo incluye una redirección genérica (por ejemplo, todo HTTP hacia su propio dominio en HTTPS), esas peticiones ajenas acaban redirigidas a él. Pueden ser dominios mal configurados, peticiones por IP o un proxy que envía un `Host` inesperado. `apache2ctl configtest` da `Syntax OK` y nada avisa.

Tres medidas lo evitan:

- **Comparar `apache2ctl -S` antes y después.** La línea `default server` de cada puerto debe ser la misma. Si cambia, se revierte antes de recargar. Es fácil automatizarlo: guardar esa línea, activar el sitio, comparar y solo entonces `systemctl reload`.
- **Nombrar el fichero para que no quede primero**, o reservar de forma deliberada un `000-default.conf` que sea siempre el predeterminado.
- **Condicionar las redirecciones al propio dominio** con `RewriteCond %{HTTP_HOST} ^(www\.)?midominio\.tld$ [NC]`, para que el sitio no actúe sobre peticiones que no son suyas aunque le lleguen.

Revisar después el registro de accesos del sitio nuevo dice si en ese intervalo llegó alguna petición ajena.

## Fuentes

- [Virtual Host Matching — Apache HTTP Server 2.4](https://httpd.apache.org/docs/2.4/vhosts/details.html)
- [Name-based Virtual Host Support — Apache HTTP Server 2.4](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)

## Relacionado

- [La cabecera Host decide qué sitio responde cuando un servidor aloja varios](La%20cabecera%20Host%20decide%20qu%C3%A9%20sitio%20responde%20cuando%20un%20servidor%20aloja%20varios.md)
- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md)
