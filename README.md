# store-api con `Node.js`: API REST con Express.js

## ¿Qué es Express.js?

Express.js es un framework de aplicaciones web minimalista y flexible escrito en JavaScript y alojado dentro del entorno de ejecución `Node.js`. Es una de las bibliotecas más populares para el desarrollo de aplicaciones web en `Node.js`, y se utiliza para crear una amplia gama de aplicaciones, desde simples sitios web hasta aplicaciones web complejas.

Express.js proporciona una serie de características que facilitan el desarrollo de aplicaciones web, incluyendo:

* Enrutamiento: Express.js proporciona una API de enrutamiento simple y potente que permite a los desarrolladores definir rutas para sus aplicaciones web.
* Manejadores de solicitudes: Express.js proporciona una serie de manejadores de solicitudes predefinidos que pueden utilizarse para manejar diferentes tipos de solicitudes HTTP.
* Módulos: Express.js utiliza módulos para organizar su código, lo que facilita la reutilización y el mantenimiento del código.

## Configuración entorno de desarrollo

Configuración inicial

* `npm init -y` ➡ crea archivo *package.json* para la configuración del entorno.
* `.editorconfig` ➡ configuraciones de nuesto editor | De esta manera todos los desarrolladores tendrán una misma configuración del editor. Para esto primero instalaremos la extensión *editorconfig*.
* `.eslintrc.json` ➡ Configuración de buenas prácticas
* `index.js` ➡ archivo inicial 

### Package.json

El archivo package.json es un archivo de configuración que se utiliza para describir un proyecto de `Node.js`. Contiene información sobre el nombre del proyecto, la versión, las dependencias, los scripts y otros datos.

El archivo package.json se utiliza para las siguientes tareas:

* Instalación de dependencias: El archivo package.json se utiliza para especificar las dependencias del proyecto. Cuando se instala un proyecto de `Node.js`, npm utiliza el archivo package.json para instalar las dependencias necesarias.
* Publicación de proyectos: El archivo package.json se utiliza para especificar la información que se utilizará al publicar un proyecto en un repositorio público, como npm.
* Gestión de versiones: El archivo package.json se utiliza para especificar la versión del proyecto. Esto permite a los desarrolladores rastrear los cambios en el proyecto y garantizar que todos los usuarios estén utilizando la misma versión.

El archivo package.json se compone de los siguientes campos:

* name: El nombre del proyecto.
* version: La versión del proyecto.
* description: Una descripción del proyecto.
* main: El archivo principal del proyecto.
* scripts: Un conjunto de scripts que se pueden ejecutar con el comando npm run.
* dependencies: Una lista de dependencias del proyecto.
* devDependencies: Una lista de dependencias de desarrollo del proyecto.
* peerDependencies: Una lista de dependencias que deben instalarse en el sistema del usuario para que el proyecto funcione correctamente.
* keywords: Una lista de palabras clave que describen el proyecto.
* author: El nombre del autor del proyecto.
* license: La licencia del proyecto.
* bugs: Información de contacto para informar de errores.
* homepage: La URL del sitio web del proyecto.

En este archivo vamos a crear algunos tareas:

1. Levantar un entorno de desarrollo ➡
    ```js
    "scripts": {
        "dev": "nodemon index.js",
        "start": "node index.js",
        "lint": "eslint"
    }
    ```
    Será necesario instalar algunas dependencias:
    `npm i nodemon eslint eslint-config-prettier eslint-plugin-prettier prettier -D`


## Instalación de Express.js y tu primer servidor HTTP

`npm i express` ➡ Instalar express como una dependencia de producción. Esto agregará una nueva dependencia en nuestro archivo package.

```js
// # tu primer servidor
const express = require('express');

//1- crear la app des express
const app = express();

//2- elegir el puerto donde corre la app
const port = 3000; //

//3- definir la ruta y qué retornar

app.get('/', (req, res) => {
  res.send('my express server is running :)');
})

//4- elige el puerto que escucha
app.listen(port, () => {
  console.log(`my port ${port} is running`);
})
```

* En el *callback* del app.get siempre tenemos al menos dos params, el `res` y el `req`. Para retornar algo al usuario, utilizamos **res**.

`req` es un objeto que contiene información sobre la solicitud HTTP que provocó el evento. En respuesta a `req`, usa respara devolver la respuesta HTTP deseada.

## Routing con Express.js

El enrutamiento es el proceso de asociar una URL con un manejador de solicitudes en una aplicación web. En Express.js, el enrutamiento se realiza utilizando la función app.route().

La función app.route() toma dos argumentos: la ruta y el manejador de solicitudes. La ruta es una cadena que representa la URL que se va a asociar con el manejador de solicitudes. El manejador de solicitudes es una función que se ejecutará cuando se reciba una solicitud a la ruta especificada.

Por ejemplo, el siguiente código define una ruta que se asocia con un manejador de solicitudes que devuelve la cadena "Hola, mundo":

```js
const express = require('express');

const app = express();

app.route('/hola', (req, res) => {
  res.send('Hola, mundo');
});

app.listen(3000);
```

Para nuestro caso de estudio, podemos agregar nuevas rutas:

```js
app.get('/', (req, res) => {
  // el retorno al cliente aquí 👈
  res.send('my express server is running :)');
})

// definir una nueva ruta
app.get('/newRoute', (req, res) => {
  res.send('This is a new route :)');
})

// retornar en formato json
app.get('/json', (req, res) => {
  res.json({
    name: 'Tony Panzas',
    age: 5,
    occupation: 'Hitman'
  });
})
```

## ¿Qué es una RESTful API?

Una API RESTful es una interfaz de programación de aplicaciones que sigue los principios de la arquitectura REST. REST es un acrónimo de Representational State Transfer, que es un estilo arquitectónico para la comunicación entre sistemas informáticos.

Una API RESTful se basa en el uso de los métodos HTTP para realizar operaciones sobre los recursos. Los recursos son entidades que se pueden identificar y acceder a través de una URL. Los métodos HTTP se utilizan para indicar la operación que se desea realizar sobre un recurso.

Los métodos HTTP más comunes utilizados en las APIs RESTful son:

| VERBO | DESCRIPCIÓN |
| ----- | ----------- |
|**GET** | Se utiliza para leer la representación de un recurso.
|**POST** | Se utiliza para crear un nuevo recurso.
|**PUT** | Se utiliza para actualizar la representación de un recurso existente.
|**DELETE** | Se utiliza para eliminar un recurso.

Además de los métodos HTTP, las APIs RESTful también utilizan el formato JSON para representar los datos. JSON es un formato de texto ligero que es fácil de leer y escribir.

## GET: recibir parámetros

Los **endpoints** son las URLs de un API o un backend que responden a una petición. Los mismos **entrypoints** tienen que calzar con un endpoint para existir. Algo debe responder para que se renderice un sitio con sentido para el visitante.

Ejemplo 1

```js
// get por id
app.get('/users/:id', (req, res) => {

  // captura los params id
  const id = req.params.id;  // ES6 => const { id } = req.params;
  
  res.json([
  {
    id,
    name: 'Nano Street',
    age: 2,
    occupation: 'Homeless'
  }]);
})
```

Si queremos hacer una búsqueda, podemos implementar el siguiente endpoint:

## GET: parámetros query

En `Node.js`, los parámetros de consulta GET son valores que se pasan a una ruta HTTP a través de la consulta de la URL. Estos valores se pueden utilizar para controlar el comportamiento de la ruta o para recuperar datos del cliente.

Por ejemplo, la siguiente ruta HTTP utiliza un parámetro de consulta GET para controlar el número de elementos que se devolverán:

<pre>GET /users?limit=10</pre>

```js
const app = express();

app.get('/users', (req, res) => {
  const limit = req.query.limit;

  // ...
});
```

En esta ruta, el parámetro de consulta limit tiene el valor 10. Este valor se puede utilizar para limitar el número de usuarios que se devolverán en la respuesta.

Los parámetros de consulta GET también se pueden utilizar para recuperar datos del cliente. Por ejemplo, la siguiente ruta HTTP utiliza un parámetro de consulta GET para recuperar el nombre del usuario:

<pre>GET /users?name=Tony&lastname=Panzas</pre>

## Separación de responsabilidades con express.Router

La Separación de Responsabilidades es un principio de diseño que aboga por dividir el código en partes distintas, cada una encargada de una funcionalidad específica. En el contexto de Express.js, express.**Router** es una herramienta que facilita la aplicación de este principio.

En `Node.js` con Express, **un enrutador (Router) es un middleware que se utiliza para organizar rutas y manejar solicitudes HTTP de manera modular**. Permite dividir la lógica de manejo de rutas en archivos o módulos separados, lo que mejora la legibilidad y facilita el mantenimiento del código.

Aquí tienes un ejemplo básico de cómo puedes usar `express.Router`:

```js
// En un archivo routes.js

const express = require('express');
const router = express.Router();

// Definir rutas y manejo de solicitudes
router.get('/', (req, res) => {
  res.send('Bienvenido a la página principal');
});

router.get('/about', (req, res) => {
  res.send('Acerca de nosotros');
});

// Exportar el router para usar en otros archivos
module.exports = router;

```
Luego, en tu archivo principal (por ejemplo, `app.js`):


```js
const express = require('express');
const app = express();

// Importar el enrutador
const routes = require('./routes');

// Usar el enrutador para ciertas rutas
app.use('/', routes);

// Otro manejo de rutas en el archivo principal
app.get('/contact', (req, res) => {
  res.send('Página de contacto');
});

// Iniciar el servidor
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Servidor en ejecución en http://localhost:${PORT}`);
});

```
