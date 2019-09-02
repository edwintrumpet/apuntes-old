# Configuración inicial del entorno de trabajo

## Índice

- [Instalación de  Hyper en Ubuntu 18.04](#titulo1)
- [Desinstalación de Hyper en Ubuntu 18.04](#titulo2)
- [Instalación de zsh y oh-my-zsh](#titulo3)
- [Instalar el plugin zsh Autosuggestions](#titulo4)

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

<a name="titulo3" />

## Instalación de zsh y oh-my-zsh

Instalamos **zsh** con el comando
```shell
sudo apt-get install zsh
```

Para instalar **oh-my-zsh** vamos a la [documentación](https://github.com/robbyrussell/oh-my-zsh) y usamos este comando que indica una instalación manual.

>   ![oh-my-zsh](https://documentacionspark.s3-sa-east-1.amazonaws.com/oh-my-zsh.png)

Nos preguntará si deseamos cambiar el shell por defecto a zsh y le diremos que sí.

Ahora para hacer que **zsh** sea el shell por defecto del sistema usaremos el comando

```shell
chsh -s /usr/bin/zsh
 ```

Para que este cambio sea efectivo debemos reiniciar el servidor, pero si deseamos usar zsh sin reiniciarlo simplemente podemos entrar usando el comando

```shell
zsh
```

Si deseamos volver al shell que teníamos por defecto antes de instalar zsh podemos hacerlo usando el comando

```shell
chsh -s /bin/bash
```

Y de nuevo, para que sea efectivo este cambio debemos reiniciar el servidor.

<a name="titulo4" />

## Instalar el plugin zsh Autosuggestions

Buscamos en Google *zsh autosuggestions* y encontraremos en los primeros lugares de la búsqueda el repositorio de GitHub.

En el README.md buscamos la sección de instalación y nos dirigimos al enlace que se indica ahí.

>![instalación autosuggestions](https://documentacionspark.s3-sa-east-1.amazonaws.com/autosuggestions-install.png)

En este archivo buscamos la sección Oh My Zsh y seguimos los pasos indicados en la siguiente imagen.

>![instalación autosuggestions](https://documentacionspark.s3-sa-east-1.amazonaws.com/autosuggetions.png)

Para esto nos en la consola nos desplazamos al directorio indicado usando el comando

```shell
cd ~/.oh-my-zsh/custom/plugins
```

Estando en el directorio indicado clonamos el repositorio con el comando

```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

```

Ahora entramos al archivo `.zshrc` para editarlo usando el comando.

```shell
vim ~/.zshrc
```

Buscamos la línea `plugins` en donde puede aparecer git como plugin preinstalado, dentro de los paréntesis le agregamos `zsh-autosuggestions` separado por un espacio del plugin que ya esté instalado asi:

```
plugins=(git zsh-autosuggestions)
```

Ahora reiniciamos la terminal y deberá autocompletarnos la escritura de acuerdo al historial.

---
[Otros contenidos](../README.md)