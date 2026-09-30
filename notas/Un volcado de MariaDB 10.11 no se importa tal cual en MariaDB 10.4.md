---
type: note
aliases:
  - utf8mb3 en MariaDB antigua
  - sandbox mode mariadb-dump
tags:
  - despliegue
  - bases-de-datos
  - mariadb
---

# Un volcado de MariaDB 10.11 no se importa tal cual en MariaDB 10.4

Preparar una base de datos en un entorno local reciente y subirla a un servidor de producción más antiguo es habitual, y el volcado falla por dos detalles que no dependen de los datos:

- **La primera línea.** Las versiones recientes de `mariadb-dump` escriben `/*M!999999\- enable the sandbox mode */` al principio del fichero. Los clientes antiguos no la entienden y abortan la importación en la línea 1.
- **El nombre `utf8mb3`.** Desde MariaDB 10.6, `utf8` se escribe en los volcados como `utf8mb3` (`CHARSET=utf8mb3`, `COLLATE=utf8mb3_general_ci`). En 10.4 ese nombre no existe y las tablas no se crean.

Las dos se arreglan con un filtro de texto antes de importar: borrar la primera línea si contiene `sandbox mode` y sustituir `utf8mb3` por `utf8` en las declaraciones de juego de caracteres y colación. Hay que limitar la sustitución a esas declaraciones, porque la cadena también puede aparecer dentro de los datos.

Lo que evita sorpresas en producción es **probar la importación en un contenedor de la misma versión que el destino** (`docker run mariadb:10.4`) y contar filas al terminar. Cuesta un minuto y descarta una importación a medias en una base compartida.

## Fuentes

- [Supported Character Sets and Collations — MariaDB Knowledge Base](https://mariadb.com/kb/en/supported-character-sets-and-collations/)
- [mariadb-dump — MariaDB Knowledge Base](https://mariadb.com/kb/en/mariadb-dump/)

## Relacionado

- [Una copia de seguridad que no sale del servidor no es una copia de seguridad](Una%20copia%20de%20seguridad%20que%20no%20sale%20del%20servidor%20no%20es%20una%20copia%20de%20seguridad.md)
- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
