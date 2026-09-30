---
type: note
aliases:
  - Cron externo
  - Planificador en proceso
tags:
  - desarrollo
  - despliegue
  - arquitectura
---

# Las tareas periódicas salen de la aplicación a un disparador externo autenticado

Meter un planificador dentro del proceso de la aplicación (`node-cron`, `APScheduler`, un `setInterval`) es lo cómodo y funciona hasta el día del primer despliegue serio. Falla de tres maneras:

1. **Con más de una réplica, la tarea se ejecuta una vez por réplica.** Tres contenedores idénticos mandan tres correos al mismo destinatario. Nadie lo prueba porque en desarrollo siempre hay una instancia.
2. **El temporizador muere con el proceso.** Un reinicio a las 7:59 se come la tarea de las 8:00 sin dejar rastro: no hay error, simplemente no pasó nada.
3. **No se puede lanzar a mano** para probar o para recuperar una ejecución perdida, salvo esperando al reloj.

El patrón alternativo separa el *cuándo* del *qué*: la aplicación expone la tarea como una **ruta HTTP** (`POST /api/cron/…`) y un disparador externo la llama según su propio horario. Ese disparador puede ser un contenedor mínimo con `crond`, el planificador del sistema operativo o el del proveedor: es intercambiable, que es justamente la ventaja.

Lo que la ruta tiene que cumplir para que el patrón no se convierta en un agujero:

- **Autenticarse ella sola**, con un secreto compartido en la cabecera `Authorization` comparado en tiempo constante. No vale apoyarse en la sesión de navegador: el llamante no tiene cookies.
- **Quedar excluida del redirector al login** que protege el resto de la aplicación, o el cliente recibirá un `302` a una página HTML y lo interpretará como éxito. Ver [El middleware no puede redirigir al login las rutas que consume un cliente no navegador](El%20middleware%20no%20puede%20redirigir%20al%20login%20las%20rutas%20que%20consume%20un%20cliente%20no%20navegador.md).
- **Ser idempotente**. Si se llama dos veces el mismo lunes, no debe duplicar los avisos. Lo normal es marcar el periodo ya procesado en la base de datos y salir sin hacer nada.
- **No estar publicada a internet**. En un despliegue con contenedores basta con que el disparador hable con la aplicación por la red interna.

El compromiso: se cambia un temporizador invisible por una superficie HTTP que hay que proteger. A cambio se gana algo que el temporizador nunca dio — **la tarea es observable**: deja registro de acceso, devuelve un código de estado y se puede reproducir con una petición.

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- [Una sonda de salud comprueba las dependencias, no que el proceso siga vivo](Una%20sonda%20de%20salud%20comprueba%20las%20dependencias%2C%20no%20que%20el%20proceso%20siga%20vivo.md)
