# Servidor Express (API rest)

## Configuración inicial

Inicio un proyecto de JavaScript usando el comando

```shell
npm init -y
```

Con esto creamos el archivo `package.json` que contiene la información del proyecto y podemos editarla para que se ajuste a las características del nuestro.

Ahora iniciamos el repositorio de Git para el control de versiones usando el comando

```shell
git init
```

Creamos el directorio `src` y dentro de él el archivo `index.js` que será la entrada principal de nuestro proyecto.

En el archivo `package.json` agregamos el script **start** como se muestra a continuación

```json
"scripts": {
    "start": "node src/index.js"
}
```

Instalamos la dependencia **Express** con el comando

```shell
npm i express -S
```

Esto generó el directorio `node_modules` con todas las dependencias, para evitar tenerlas en el control de versiones creamos el archivo `.gitignore` y en él agregamos la línea `node_modules/`.

En el archivo `index.js` importamos la dependencia **Express** y creamos el servidor con una ruta inicial de prueba

```javascript
const express = require('express')

const app = express()


// Routes
app.get('/', (req, res)=>{
    res.status(200).json({message: 'Express Works!'})
})

// Server online
app.listen(3000, () => {
    console.log(`Server on port 3000`)
})
```

Ahora iniciamos el servidor con el comando

```shell
npm start
```

Y esto nos muestra el mensaje **_Server on port_** por consola y un mensaje en formato json en la ruta `http://localhost:3000`

Si realizamos algún cambio debemos detener el servidor y volver a iniciarlo para ver cada cambio, eso es un poco incómodo en el desarrollo y para agilizar vamos a instalar la dependencia de desarrollo **nodemon** con el comando

```shell
npm i nodemon -D
```

También vamos a agregar el script **dev** al `package.json` así

```json
"scripts": {
    "start": "node src/index.js",
    "dev": "nodemon src/index.js"
}
```

Y ahora para levantar el servidor en modo de desarrollo usaremos el comando

```shell
npm run dev
```

Ahora en la raíz del proyecto junto con el directorio `src` vamos a crear el directorio `config` y dentro de él un archivo `index.js` donde almacenaremos las configuraciones de nuestro proyecto y así tener un lugar central en donde poder configurar todo sin tocar el código.

Este directorio también lo vamos a agregar en el archivo `.gitignore` por seguridad ya que ahí almacenaremos contraseñas y configuraciones de seguridad que en caso de que publiquemos la aplicación en un repositorio público quedarían expuestas.

En este archivo que creamos vamos a exportar un objeto con varios valores que se usarán en el proyecto, vamos a empezar con el puerto

```javascript
module.exports = {
    SERVER_PORT: process.env.port || 3000
}
```

Ahora en el archivo `src/index.js` vamos a importar esta configuración y la usaremos en lugar de usar los valores fijos que teníamos

```javascript
const express = require('express')
const { SERVER_PORT } = require('../config')

const app = express()

// Routes
app.get('/', (req, res)=>{
    res.status(200).json({message: 'Express Works!'})
})

// Server online
app.listen(SERVER_PORT, () => {
    console.log(`Server on port ${SERVER_PORT}`)
})
```
Ahora para organizar un poco más nuestro proyecto vamos a crear en la raíz del proyecto el directorio `routes` y dentro de él un archivo `index.js` que contendrá las rutas de nuestra API, en este archivo vamos a exportar una función a la que le pasaremos el parámetro **app** de *Express* para que pueda crear las rutas

```javascript
module.exports = (app) => {
    app.get('/', (req, res) => {
        res.status(200).json({message: 'Routes Works!'})
    })
}
```

Ahora en el archivo `src/index.js` borramos la ruta *get* que teníamos y la reemplazamos por la importación de este archivo pasándole como parámetro **app**

```javascript
const express = require('express')
const { SERVER_PORT } = require('../config')

const app = express()

// Routes
require('../routes')(app)

// Server online
app.listen(SERVER_PORT, () => {
    console.log(`Server on port ${SERVER_PORT}`)
})
```

Agregamos algunos middlewares para que podamos manejar fácilmente los objetos json que recibamos por medio de las peticiones.

```javascript
const express = require('express')
const { SERVER_PORT } = require('../config')

const app = express()

// Middlewares
app.use(express.urlencoded({extended: false}))
app.use(express.json())

// Routes
require('../routes')(app)

// Server online
app.listen(SERVER_PORT, () => {
    console.log(`Server on port ${SERVER_PORT}`)
})
```

Si queremos observar por consola las peticiones realizadas para tener un poco más de control podríamos usar la dependencia **morgan** que la podemos instalar con el comando

```shell
npm i morgan -S
```

Para usarlo lo importamos en el archivo `src/index.js` y lo usamos como middleware

```javascript
const express = require('express')
const morgan = require('morgan')
const { SERVER_PORT } = require('../config')

const app = express()

// Middlewares
app.use(morgan('dev'))
app.use(express.urlencoded({extended: false}))
app.use(express.json())

// Routes
require('../routes')(app)

// Server online
app.listen(SERVER_PORT, () => {
    console.log(`Server on port ${SERVER_PORT}`)
})
```

Con esto tendríamos una API rest sencilla a la que le podemos agregar más configuraciones y rutas.

## Deploy en una instancia Lightsail de AWS

En este momento si usamos el comando `npm start` se levanta el servidor pero si cerramos la consola el servidor se cae, ese comportamiento no podemos aceptarlo en el deploy porque necesitamos la aplicación funcionando permanentemente.

Por eso vamos a modificar el script **start** para usar la aplicación pm2 que instalaremos en el servidor

```json
"scripts": {
    "start": "pm2 start src/index.js",
    "dev": "nodemon src/index.js"
}
```

Ahora creamos un repositorio en GitHub en el que vamos a subir este proyecto para hacer el deploy desde ahí.

En la consola de administración de AWS, en la pestaña de servicios, la sección de informática podremos ver **Lightsail**.

Aquí elegimos la opción *crear una instancia*

Podemos elegir la región para usar tal vez la que esté más cercana al lugar desde donde tendrá acceso la aplicación y la zona.

![Región AWS](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/regiosaws.png)

En la selección de imagen de la instancia elegimos una plataforma Linux con Nodejs

![imagen instancia](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/imageninstancia.png)

En seguida aparece una opción para usar una llave SSH que tengamos o crear una nueva para la conexión de manera remota y segura con la instancia.

![llave ssh](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/llavessh.png)

Creamos una nueva llave, le ponemos un nombre y la descargamos en un lugar seguro.

Esta llave sólo la podemos descargar una vez, si la perdemos no la podremos recuperar y tendremos que generar una nueva.

Elegimos la instancia de nuestra preferencia, para una prueba podría ser la más pequeña en la cual el primer mes es gratis.

![Precios aws](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/preciosaws.png)

Le ponemos un nombre para identificar la instancia y presionamos el botón de crear instancia.

![nombre](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/nombreinstancia.png)

Como resultado obtendríamos la instancia y aquí podemos ver la ip pública con la que podemos acceder desde el navegador, y algunas características de nuestra instancia.

![instancia](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/instanciacuadro.png)

El ícono rectangular de la parte superior derecha de la tarjeta abre una terminal online desde la que tenemos acceso a nuestra instancia y en los puntos algunas opciones adicionales.

Si damos click en el nombre podemos ver además el nombre de usuario que podemos usar para la conexión SSH desde nuestra terminal.

![usuario](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/usuarioinstancia.png)

**Conexión SSH desde Ubuntu**

Primero vamos a mover la llave que descargamos cuando creamos la instancia en el directorio `.ssh/`.

Mi llave se llama `llave.pem` y la guardé en la carpeta de Descargas por lo que usaré el comando

```shell
mv ~/Descargas/llave.pem ~/.ssh
```

Ahora debemos cambiar los permisos de la llave para que no genere errores al intentar una conexión

```shell
sudo chmod 600 llave.pem
```

Con esto debería ser suficiente para establecer la conexión, pero vamos a configurar un alias para no tener que recordar la IP y el nombre de la llave cada vez que nos conectemos.

Abrimos el archivo `.ssh/config` con el comando

```shell
vim ~/.ssh/config
```

Y lo editamos con la siguiente configuración usando en *Host* el alias que queremos usar, en *Hostname* la IP pública del servidor, en *User* el usuario que tenemos en el servidor y en *IdentityFile* la ubicación de la llave

```
Host instanciaprueba
    Hostname 18.209.230.41
    Port 22
    User bitnami
    IdentityFile ~/.ssh/llave.pem
```

Si queremos agregar nuevos alias para otras conexiones SSH podemos hacerlo en nuevas líneas usando el mismo procedimiento, ahora podemos acceder de forma remota al servidor usando el comando

```shell
ssh instanciaprueba
```

Lo primero que hago al conectarme es actualizar el software con los comandos

```shell
sudo apt-get update
```
```shell
sudo apt-get upgrade
```

Suponiendo que el repositorio en el que tengo almacenada la aplicación es privado y necesito conectarme a él de manera segura, entonces voy a crear un par de llaves para conectarme por SSH a GitHub con el comando

```shell
ssh-keygen -t rsa -b 4096
```

Le dejamos el nombre y la ubicación por defecto presionando enter y si queremos podemos darle un passphrase o dejarlo así

Para evaluar que el servidor SSH esté corriendo usamos el comando

```shell
eval $(ssh-agent -s)
```

Y para finalizar agregamos la llave generada al archivo `known_host` con el comando

```shell
ssh-add ~/.ssh/id_rsa
```

Con esto nos dirigimos al repositorio en GitHub y en la parte de configuraciones buscamos la pestaña de llaves de Deploy

![llaves de deploy](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/deploygithub.png)

En la terminal del servidor podemos ver la llave pública con el comando

```shell
cat ~/.ssh/id_rsa.pub
```

Seleccionamos todo el texto y lo copiamos con la combinación de teclas **`Ctrl`** + **`Shitf`** + **`c`**. Tener cuidado de no usar **`Ctrl`** + **`c`** que en lugar de copiar nos hará salir de la terminal del servidor.

Presionamos el botón *Add deploy key*, le ponemos un título a la llave para distinguir su procedencia y pegamos el texto en el campo *key*.
Podemos habilitar la escritura pero no es importante para el proceso que queremos hacer que es clonar, entonces simplemente agregamos la llave.

Ahora podemos clonar un repositorio privado desde nuestro servidor.

Para levantarlo instalamos **pm2** de manera global con el comando

```shell
npm i pm2 -g
```

Entramos al directorio donde está nuestra aplicación e instalamos las dependencias.

```shell
npm i
```
Recordemos que cuando hicimos los commits no incluimos el directorio de configuración por lo que lo vamos a crear. Estando en el directorio de la API usamos el comando

```shell
mkdir config
```
Para crear el directorio de configuraciones, ingresamos en él y usamos el comando

```shell
vim index.js
```

Para crear el archivo de configuraciones que editaremos igual al que teníamos en el archivo local.

Ahora para levantar el servidor usamos el mismo comando

```shell
npm start
```

Para que Apache redirija nuestra aplicación por el puerto http de salida vamos a editar el siguiente archivo

```shell
vim /opt/bitnami/apache2/conf/bitnami/bitnami-app-prefix.conf
```

En el que escribiremos estas líneas

```
ProxyPass / http://127.0.0.1:3000/
ProxyPassReverse / http://127.0.0.1:3000/
```

Ahora podemos ir al navegador y con la IP pública de nuestro servidor acceder a la API rest

## Apuntar un Dominio de Namecheap a esta instancia

Para apuntar un dominio de namecheap vamos al inicio de la web de Amazon Lightsail donde están nuestras instancias y en la pestaña Networking elegimos la opción Crear zona DNS.

![crear zona dns](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/creardns.png)

Aquí ingresamos el nombre del dominio que tenemos y continuamos.

Ahora vemos una opción de crear record y cuatro nombres de servidores.

Presionamos en *add record* usamos la opción **A record** como subdominio ponemos **www** y resolvemos a la instancia que la podemos elegir entre las opciones.

Ahora vamos a el sitio web de *Namecheap* y en la lista de dominios que tenemos en la parte derecha del dominio elegido entramos en la opción de administrar.

En la sección de *NAMESERVERS* ponemos la opción *Custom DNS* y pegamos los nombres de dominios que habían en AWS

![servidores](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/servidores.png)

![namecheap](https://documentacionspark.s3-sa-east-1.amazonaws.com/nodejs/namecheap.png)

Ahora solo esperamos a que actualicen los DNS y podremos acceder usando nuestro dominio a nuestra API