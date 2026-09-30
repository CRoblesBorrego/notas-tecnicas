---
type: note
aliases:
  - Cron como root en un contenedor
  - Propiedad de los ficheros creados por cron
tags:
  - despliegue
  - sistemas
  - contenedores
---

# Una tarea periódica debe correr como el usuario del servicio

Sacar las tareas programadas de una aplicación y llevarlas al cron del sistema es buena práctica: dejan de depender de que alguien visite la web y de un endpoint accesible desde internet. Pero la línea del `crontab` la escribe root, y si no se dice lo contrario, **las tareas se ejecutan como root**.

Mientras la tarea solo lea, no pasa nada. El día que **crea un fichero o un directorio**, ese objeto queda con dueño root, y el proceso del servidor web —que corre como otro usuario— pierde el acceso de escritura.

El caso que lo revela suele ser estacional y por eso desconcierta. Una aplicación que archiva las subidas por año y mes necesita una carpeta nueva el día 1 de cada mes. La crea quien llegue primero. Si el cron se adelanta a la primera publicación, la carpeta nace de root y **a partir de ese momento fallan todas las subidas del mes**, con un error que habla de mover ficheros y no menciona permisos ni cron. El síntoma aparece el día 1, desaparece del radar hasta el mes siguiente, y no se parece en nada a la causa.

La corrección es una opción en la propia línea del cron: ejecutar la tarea con el usuario del servicio. Con contenedores, eso significa indicarlo explícitamente al entrar en el contenedor, porque por omisión se entra como root aunque el proceso principal corra como otro usuario.

Y conviene dejarlo escrito **en el propio script que da de alta el cron**, no en la documentación: quien lo reinstale dentro de seis meses no va a leer la nota, pero sí el comentario que tiene delante.

Tres señales de que se está ante este problema:

- El fallo empieza en una fecha redonda —día 1, año nuevo— sin que nadie haya desplegado nada.
- Afecta a la escritura, nunca a la lectura.
- El mensaje de error habla del último paso de la operación, no del permiso que falta.

## Relacionado

- [WordPress descarta la escritura directa cuando PHP corre como root](WordPress%20descarta%20la%20escritura%20directa%20cuando%20PHP%20corre%20como%20root.md)
- [Las tareas periódicas salen de la aplicación a un disparador externo autenticado](Las%20tareas%20peri%C3%B3dicas%20salen%20de%20la%20aplicaci%C3%B3n%20a%20un%20disparador%20externo%20autenticado.md)
- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
