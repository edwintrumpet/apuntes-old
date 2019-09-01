# Configuración inicial del entorno de trabajo

## Tabla de contenidos

- [Instalación de  Hyper en Ubuntu 18.04](#titulo1)
- [Desinstalación de Hyper en Ubuntu 18.04](#titulo2)

<a name="titulo1" />

## Instalación de Hyper en Ubuntu 18.04

- En la página oficial de [Hyper](https://hyper.is) entre las opciones de descarga se elige la versión para Debian que proporcionará un archivo (.deb).
- Ubicamos la terminal en la carpeta de descargas o en donde esté el archivo descargado y con el siguiente comando realizamos la instalación.
```shell
sudo dpkg -i <paquete.deb>
```
- Si nos muestra un mensaje de error en la instalación por falta de algunas librerías podemos completar la instalación usando el comando
```shell
sudo apt -f install
```
- Con eso ya queda completamente instalada la aplicación y lista para usar.

<a name="titulo2" />

## Desinstalación de Hyper en Ubuntu 18.04

Para desinstalar Hyper usamos el comando
```shell
sudo apt-get --purge remove hyper
```
Eso desinstalará Hyper pero algunas dependencias que se instalaron junto con él seguirán instaladas, para desinstalar esas dependencias que ya no se están usando usamos el comando.
```shell
sudo apt autoremove
```