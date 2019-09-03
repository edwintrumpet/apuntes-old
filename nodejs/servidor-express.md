# Servidor Express (API rest)

## Configuración inicial

Inicio un proyecto de JavaScript usando el comando

```shell
npm init -y
```

Con esto creamos el archivo `package.json` que contiene la información del proyecto y podemos editarla para que se ajuste a las carácterísticas del nuestro.

Ahora iniciamos el repositorio de Git para el control de versiones usando el comando

```shell
git init
```

Creamos el directorio `src` y dentro de él el archivo `index.js` que será la entrada principal de nuestro proyecto.

En el archivo `package.json` agregamos el script **start** como se muestra acontinuación

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

Si realizamos algún cambio debemos detener el servidor y volver a iniciarlo para ver cada cambio, eso es un poco incómodo en el desarrollo y para agilizarlo vamos a instalar la dependencia de desarrollo **nodemon** con el comando

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
Ahora para oragnizar un poco más nuestro proyecto vamos a crear en la raíz del proyecto el directorio `routes` y dentro de él un archivo `index.js` que contendrá las rutas de nuestra API, en este archivo vamos a exportar una función a la que le pasaremos el parámetro **app** de *Express* para que pueda crear las rutas

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

Con eso ya tendríamos una configuración inicial para una API rest en Nodejs