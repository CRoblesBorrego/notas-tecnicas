---
type: note
aliases:
  - Copias de seguridad
  - Regla 3-2-1
tags:
  - despliegue
  - continuidad
  - ciberseguridad
---

# Una copia de seguridad que no sale del servidor no es una copia de seguridad

Montar el volcado automático de la base de datos es fácil: un contenedor auxiliar con las herramientas del motor, una tarea programada que hace `pg_dump | gzip` a un directorio montado, y borrado de lo más antiguo con un `find -mtime`. Lo fácil da una falsa sensación de estar cubierto, porque deja **cuatro cosas sin resolver**.

**Uno: la copia comparte destino con el original.** Si el servidor se pierde —disco, cifrado por ransomware, borrado del proveedor—, se pierden las dos a la vez. Una copia en el mismo anfitrión protege del error humano y de la corrupción lógica, que no es poco, pero no es respaldo. Solo cuenta como tal cuando **sale de la máquina** y, mejor, del mismo dominio de autenticación: si las credenciales que comprometen el servidor también borran el destino remoto, el atacante hace las dos cosas.

**Dos: la retención tiene que estar acotada, y ser suficiente.** Sin borrado, el disco se llena y la aplicación cae — la copia de seguridad provoca la incidencia que debía mitigar. Con retención demasiado corta, un daño que se descubre tarde (una corrupción silenciosa, un borrado del mes pasado) ya no tiene punto de retorno. La ventana debe cubrir el tiempo realista de **detección**, no el de ejecución del volcado.

**Tres: una copia nunca restaurada es una hipótesis.** El guion de restauración es parte del respaldo, y tiene dos exigencias no obvias: **recrear el esquema desde cero** —restaurar encima deja tablas y filas viejas que ya no están en el volcado y produce un estado que jamás existió— y **exigir confirmación explícita**, porque destruye los datos actuales.

**Cuatro: el volcado hereda la clasificación de los datos.** Un fichero con toda la base en claro, con permisos de lectura amplios, es la vía de exfiltración más cómoda que existe. Cifrado en reposo y acceso restringido, como el original.

Esto es lo que el control de copias de seguridad de los controles tecnológicos del Anexo A pide de verdad, y lo que decide si la fase de recuperación de un incidente dura horas o semanas.

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- Contener antes de erradicar, erradicar antes de recuperar
