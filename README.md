## Backwarden

Autor: Alarti

Este crea copias de seguridad cifradas para bóvedas de [Bitwarden](https://bitwarden.com/), incluyendo los adjuntos. Extrae los elementos de tu bóveda mediante el [CLI de Bitwarden](https://github.com/bitwarden/cli) y descarga todos los adjuntos asociados a una carpeta temporal de respaldo. Luego, Backwarden comprime esa carpeta, la cifra con una contraseña y elimina la carpeta temporal.

Backwarden resuelve la necesidad planteada en el foro de la comunidad ([ver discusión](https://community.bitwarden.com/t/encrypted-export/235)), aunque esperamos que pronto Bitwarden ofrezca una solución oficial.

## 28/3/2020 Actualización

Ya es posible restaurar copias de seguridad en cuentas vacías, incluidos los adjuntos.

## Aviso Legal

Ten en cuenta que **puedes perder tus datos** si utilizas la función de restauración y no me hago responsable de lo que ocurra. Usa este software libre bajo tu propio criterio. Como Backwarden no gestiona conflictos de restauración, **asegúrate de hacer la copia de seguridad con tu cuenta principal y restaurar solo en una cuenta alternativa/suplementaria**.

## Uso del CLI de Backwarden

Visita https://github.com/bitwarden/cli/releases para descargar la última versión del CLI de Bitwarden y coloca el ejecutable `bw`/`bw.exe` en tu `PATH`. Después, descarga la última versión de Backwarden desde https://github.com/vwxyzjn/portwarden/releases/ (sustituye portwarden por Backwarden). Sigue estos pasos:

```bash
# Si usas una instancia self-hosted, ejecuta:
bw config server https://MYSERVER.COM

backwarden --passphrase 1234 --filename backup.backwarden encrypt
backwarden --passphrase 1234 --filename backup.backwarden decrypt
# ¡LA RESTAURACIÓN ES EXPERIMENTAL! PODRÍAS PERDER DATOS
# SI RESTAURAS EN TU CUENTA PRINCIPAL.
# ASEGÚRATE DE ENTENDER LOS RIESGOS Y UTILIZA UNA CUENTA ALTERNATIVA.

# Backwarden no resuelve conflictos, así que usa una cuenta vacía para restaurar.

backwarden --passphrase 1234 --filename backup.backwarden restore
```
### Demostración: Respaldo

![alt text](./imgs/backup.gif "Backwarden CLI Demo")

### Demostración: Descifrado

![alt text](./imgs/decrypt.gif "Backwarden CLI Demo")

### Demostración: Restauración

![alt text](./imgs/restore.gif "Backwarden CLI Demo")


## Backwarden comparado con el backup oficial de Bitwarden (a fecha 05/12/2018)
|              | Backwarden                    | Bitwarden oficial                         |
|:-------------|:-----------------------------|:------------------------------------------|
|Backend       | golang                        | C#                                        |
|Formato backup| :heavy_check_mark: Cifrado AES `.backwarden` | CSV sin cifrar              |
|Adjuntos      | :heavy_check_mark: Soportado  | No soportado (ver [petición de función](https://community.bitwarden.com/t/allow-attachments-to-be-exported-when-using-export-data)) |
|Restaurar adjuntos|:heavy_check_mark: Soportado| No soportado                             |

## Contribución y desarrollo

Clona este repositorio. Asegúrate de tener [Docker](https://docs.docker.com/install/), puertos 8000, 8081 y 5000 libres, [Golang](https://golang.org/) y [dep](https://golang.github.io/dep/) instalados. Crea una variable de entorno `Salt` de 30 caracteres para el cifrado. Luego ejecuta:

```bash
dep ensure           # Instala dependencias Go
docker-compose up -d # Arranca los contenedores requeridos

docker ps # Comprueba los contenedores activos

docker exec -it backwarden_scheduler_1 bash # Accede al contenedor scheduler
```

Observa que el archivo `docker-compose.yaml` define los servicios y mapea tu directorio actual como volumen en el contenedor. Así puedes desarrollar localmente y ejecutar en el entorno aislado. Los puertos estarán accesibles para probar los endpoints.

Las PRs son bienvenidas. Para ideas, podrías añadir una barra de progreso al CLI.

[1] chrome://newtab/
