---
type: moc
title: Despliegue con contenedores (MOC)
aliases:
  - MOC Despliegue
  - MOC Contenedores
tags:
  - moc
  - despliegue
  - docker
---

# Despliegue con contenedores — Mapa de contenido

Poner una aplicación web en un servidor propio con Docker Compose: lo que se rompe entre el `docker build` que funciona en el portátil y el servicio que aguanta un lunes por la mañana. Son lecciones de mecánica de despliegue, no de una pila concreta — el motor de base de datos o el framework cambian, los modos de fallo no.

- [Construir la imagen de una app no debe requerir la base de datos](Construir%20la%20imagen%20de%20una%20app%20no%20debe%20requerir%20la%20base%20de%20datos.md) — por qué un build hermético es un requisito y no una virtud, y el fallo de Next.js al importar todos los módulos de ruta.
- [Publicar un puerto de Docker se salta el cortafuegos del anfitrión](Publicar%20un%20puerto%20de%20Docker%20se%20salta%20el%20cortafuegos%20del%20anfitri%C3%B3n.md) — Docker desvía los paquetes antes de las cadenas que usa `ufw`; atar al bucle local y poner delante un proxy inverso.
- [Una sonda de salud comprueba las dependencias, no que el proceso siga vivo](Una%20sonda%20de%20salud%20comprueba%20las%20dependencias%2C%20no%20que%20el%20proceso%20siga%20vivo.md) — `200` frente a `503`, no cachear, no filtrar información, y por qué mezclar *vivo* con *listo* convierte un corte en una caída.
- [Las tareas periódicas salen de la aplicación a un disparador externo autenticado](Las%20tareas%20peri%C3%B3dicas%20salen%20de%20la%20aplicaci%C3%B3n%20a%20un%20disparador%20externo%20autenticado.md) — el planificador en proceso se duplica con las réplicas y muere con el despliegue; la ruta HTTP idempotente que lo sustituye.
- [Una copia de seguridad que no sale del servidor no es una copia de seguridad](Una%20copia%20de%20seguridad%20que%20no%20sale%20del%20servidor%20no%20es%20una%20copia%20de%20seguridad.md) — destino, retención, restauración probada y clasificación del volcado.
- [Una tarea periódica debe correr como el usuario del servicio](Una%20tarea%20peri%C3%B3dica%20debe%20correr%20como%20el%20usuario%20del%20servicio.md) — el cron lo escribe root, y el día que la tarea crea un directorio deja al servidor web sin escritura; un fallo estacional que aparece el día 1 y no menciona permisos.
- [Git Bash reescribe las rutas absolutas que se pasan a un contenedor](Git%20Bash%20reescribe%20las%20rutas%20absolutas%20que%20se%20pasan%20a%20un%20contenedor.md) — `ENOENT` con `Program Files/Git` en la ruta; `MSYS_NO_PATHCONV=1` para esa orden.
- [Un volcado de MariaDB 10.11 no se importa tal cual en MariaDB 10.4](Un%20volcado%20de%20MariaDB%2010.11%20no%20se%20importa%20tal%20cual%20en%20MariaDB%2010.4.md) — la línea «sandbox mode» y el nombre `utf8mb3`; probar la importación en un contenedor de la versión de destino.

## Hilo conductor

Las cinco notas comparten una forma: **el comportamiento por defecto es cómodo y silencioso, y el fallo aparece solo en producción**. Publicar un puerto, construir un cliente al importar el módulo, programar una tarea dentro del proceso o volcar la base al disco de al lado son decisiones que nadie toma explícitamente; se heredan del ejemplo mínimo. Revisarlas una por una antes de exponer el servicio cuesta una tarde y evita el tipo de incidencia que no da ningún aviso previo.

## Relacionado

- [Desarrollo web (MOC)](Desarrollo%20web%20%28MOC%29.md) — lo que pasa dentro de la aplicación.
- [Índice de Notas](../README.md)
