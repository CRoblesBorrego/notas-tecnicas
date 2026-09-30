---
type: note
aliases:
  - MSYS_NO_PATHCONV
  - Conversión de rutas de MSYS
tags:
  - docker
  - windows
  - linea-de-ordenes
---

# Git Bash reescribe las rutas absolutas que se pasan a un contenedor

En Windows, Git Bash (MSYS2) **convierte automáticamente los argumentos que parecen rutas POSIX absolutas** en rutas de Windows antes de pasarlos a un programa nativo. La intención es buena —que `notepad /c/archivo.txt` funcione—, pero rompe cualquier orden que lleve una ruta pensada para *dentro* de un contenedor:

```
docker compose exec servicio programa --input=/datos/flujo.json
```

llega al contenedor como `--input=C:/Program Files/Git/datos/flujo.json`, y el programa responde `ENOENT` con una ruta que nadie escribió. El mensaje despista porque parece un problema del volumen montado, y el archivo sí está en el contenedor.

Arreglos:

- **Desactivar la conversión para esa orden**: `MSYS_NO_PATHCONV=1 docker compose exec …` (o `export` en el script).
- **Doblar la barra inicial** (`//datos/flujo.json`), que MSYS deja como está. Funciona, pero es menos legible.
- Ejecutar la orden desde PowerShell o `cmd`, que no reescriben nada.

Pista para reconocerlo: cualquier ruta de error que contenga `Program Files/Git` en un programa que corre en Linux es esta conversión.

## Fuentes

- [Filesystem Paths — MSYS2](https://www.msys2.org/docs/filesystem-paths/)

## Relacionado

- [Despliegue con contenedores (MOC)](Despliegue%20con%20contenedores%20%28MOC%29.md)
- Windows reserva rangos enteros de puertos y el puerto parece libre
