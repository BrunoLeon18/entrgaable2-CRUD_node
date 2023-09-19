# TODO API

## Levantar Proyecto

```shell
npm init -y
```

Instalar las dependencias del proyecto
```shell
npm i express sequelize cors pg pg-hstore cors dotenv
```

Instalar las dependencias de desarrollo
```shell
npm i nodemon -D
```

# Modificar el Package.json

1 agregar el soporte para es6 -> 
```json
"type":"module"
```

2.- Agregar los scrips de arranque y desarrollo
```json
 "scripts": {
    "start": "node ./src/app.js",
    "dev":"nodemon ./src/app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```

```json
{
  "name": "associations",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "pg": "^8.11.3",
    "pg-hstore": "^2.3.4",
    "sequelize": "^6.32.1"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

## Crear nuestras carpetas y archivos

-/
  -/src
    -/models
    -/app.js
 -.gitignore
 -.env   

 En .gitignore agregamos lo siguiente
```
 node_modules
 .env
 ```
## Crear un servidor Basico

Nos dirigimos al archivo app.js

1 importamos express

```js
import express from "express";
```

2.- Crear una instancia de express

```js
import express from "express";

const app = express();
```

3.- Creamos una ruta para healtCheck

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("OK");
});
```

4.- Creamos una variable para nuestro Puerto

```js
import express from "express";

const PORT = process.env.PORT ?? 8000;

const app = express();

app.get("/", (req, res) => {
  res.send("OK");
});
```

5.- Dejar escuchando al servidor en el puerto

```js
import express from "express";

const PORT = process.env.PORT ?? 8000;

const app = express();

app.get("/", (req, res) => {
  res.send("OK");
});

app.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});

```

## Conectarnos a la base de datos


1.- Crear una base de datos en postgres. (todo_db)

2.- En utils crear un archivo database.js

3.- Importar Sequelize y dotenv



```js
import { Sequelize } = from 'sequelize';
import 'dotenv/config';
```

4.- Crear una isntancia con la información de la conexión

```js
import { Sequelize } = from 'sequelize'
import 'dotenv/config';

const db = new Sequelize({
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  port: process.env.DB_PORT,
  username: process.env.D,
  password: 'root'
});


```
5.- exportar la configuracion de la conexion

```js
import { Sequelize } from 'sequelize'
import 'dotenv/config';

const db = new Sequelize({
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  port: process.env.DB_PORT,
  username: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  dialect: "postgres",
  dialectOptions: {ssl: {required: true, rejectUnauthorized: false}}
});

export default  db;
```

### Probar la conexión a la base de datos


En app.js importamos db para usar el método authenticate

```js
import express from "express";
import db from "./utils/database.js";

const PORT = process.env.PORT ?? 8000;

const app = express();

app.get("/", (req, res) => {
  res.send("OK");
});

app.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});
```

Usamos el método authenticate para probar la conexión

```js
import express from "express";
import db from "./utils/database.js";

db.authenticate()
  .then(() => console.log("Base de datos conectada correctamente"))
  .catch((e) => console.log(e));

const PORT = process.env.PORT ?? 8000;

const app = express();

app.get("/", (req, res) => {
  res.send("OK");
});

app.listen(PORT, () => {
  console.log(`Servidor escuchando en el puerto ${PORT}`);
});
```

## Crear el modelo

1.- En models crear un archivo todo.model.js
2.- importar Datatypes y db

```js
import { DataTypes } from "sequelize";
import db from "../utils/database.js";
```

3.- Definir el modelo con los atributos que queremos guardar:

```js
import { DataTypes } from "sequelize";
import db from "../utils/database.js";


const Todo = db.define('todos',{
    id: {
        type : DataTypes.INTEGER,
        primaryKey: true ,
        autoIncrement:true
    },
    
    title:{
        type:DataTypes.STRING(50),
        allowNull: false
    },

    description:{
        type:DataTypes.STRING(200),
        allowNull: false
        },

    completed:{
        type:DataTypes.BOOLEAN(),
        defaultValue: false,
        allowNull: false
    }
})

```

4.- exportar el modelo

```js
import { DataTypes } from "sequelize";
import db from "../utils/database.js";


const Todo = db.define('todos',{
    id: {
        type : DataTypes.INTEGER,
        primaryKey: true ,
        autoIncrement:true
    },
    
    title:{
        type:DataTypes.STRING(50),
        allowNull: false
    },

    description:{
        type:DataTypes.STRING(200),
        allowNull: false
        },

    completed:{
        type:DataTypes.BOOLEAN(),
        defaultValue: false,
        allowNull: false
    }
})

export default Todo;

```

## sincronizacion de la base de datos 

1.-usamos el metodo sync para sincronizar la base de datos

```js
import express from 'express';
import db from './utils/database.js';


Todo;
const app = express()
const PORT = process.env.PORT ?? 8000
app.use(express.json())


db.authenticate()
    .then( () => console.log('conexion correcta'))
    .catch( (err) => console.log(err))

db.sync()
    .then(()=> console.log('base de datos sincronizada'))
    .catch((err)=> console.log(err))


app.get('/', (req, res)=>{
    res.send('OK')
})

```

## creacion de metodos http

### GET

1.- importamos el modelo todo.model.js en nuestro archivo app.js y lo ejecutamos:

```js
import express from 'express';
import db from './utils/database.js';
import Todo from './models/todo.model.js';


Todo;

```
2.- creamos el metodo GET en nuestro archivo app.js

```js
app.get('/todos', async (req, res)=>{
 
})
```

2.1.- colocamos un try/catch para la asincronia

```js
app.get('/todos', async (req, res)=>{
    try {
       
    } catch (error) {

    }
})
```
2.2.- usamos el metodo findAll para llamar a todos y enviamos los resultados con json
```js

app.get('/todos', async (req, res)=>{
    try {
        const todos = await Todo.findAll()
        res.json(todos);
    } catch (error) {

    }
})
 ```

 2.3.- enviamos la respuesta del error
 ```js
 app.get('/todos', async (req, res)=>{
    try {
        const todos = await Todo.findAll()
        res.json(todos);
    } catch (error) {
        res.status(400).json(error)
    }
})

```
### POST

3.- Creamos el metodo POST en nuestro archivo app.js

```js
app.post('/todos', async (req, res) => {
     //aqui va todo lo que se ejecutara cuando reciba una peticion tipo POST
})
```

3.1.- colocamos un try/catch para la asincronia

```js
app.post('/todos', async (req, res)=>{
    try {
       
    } catch (error) {

    }
})
```
3.2.- destructuramos el req.body con la variable body y lo ejecutamos con el metodo create, 
    tambien enviamos la respuesta de la peticion en json

```js

app.post('/todos', async (req, res)=>{
    try {
        const { body } = req
        const todo = await Todo.create(body)
        res.status(201).json(todo)
    } catch (error) {

    }
})
 ```

 3.3.- enviamos la respuesta del error

 ```js
 app.post('/todos', async (req, res)=>{
     try {
        const { body } = req
        const todo = await Todo.create(body)
        res.status(201).json(todo)
    } catch (error) {
        res.status(400).json(error)
    }
})

```
### UPDATE

3.- Creamos el metodo UPDATE en nuestro archivo app.js

```js
app.put('/todos/:id', async (req, res) => {
    //aqui va todo lo que se ejecutara cuando reciba una peticion tipo UPDATE
})
```

3.1.- colocamos un try/catch para la asincronia

```js
app.put('/todos/:id', async (req, res) => {
    try {
        
    } catch (error) {

    }
})
```
3.2.- destructuramos el req.body con la variable body y el req.params con la variable 
    id, con el metodo update ejecutamos el body y el objeto con la condicion where
    indicando que tiene que haber un id en la peticion, por ultimo enviamos la respuesta de la peticion
```js

app.put('/todos/:id', async (req, res) => {
    try {
       const { id } = req.params
        const { body } = req
        const todo = await Todo.update(body, {
            where: {id}
        })
        res.json(todo)
    } catch (error) {

    }
})
 ```

 3.3.- enviamos la respuesta del error

 ```js
app.put('/todos/:id', async (req, res) => {
    try {
        const { id } = req.params
        const { body } = req
        const todo = await Todo.update(body, {
            where: {id}
        })
        res.json(todo)
    } catch (error) {
        res.status(400).json(error)
    }
})  

```

### DELETE

4.- Creamos el metodo DELETE en nuestro archivo app.js

```js
app.delete('/todos/:id', async (req, res) => {
    //aqui va todo lo que se ejecutara cuando reciba una peticion tipo DELETE
})
```

4.1.- colocamos un try/catch para la asincronia

```js
app.delete('/todos/:id', async (req, res) => {
    try {
        
    } catch (error) {

    }
})
```
4.2.- destructuramos el req.params con la variable id para luego mandarla a llamar el
    metodo destroy en la condicion where, al final mandamos la respusta de la peticion
    y la finalizamos con el metodo .end()

```js

app.delete('/todos/:id', async (req, res) => {
    try {
       const { id } = req.params
        const { body } = req
        const todo = await Todo.update(body, {
            where: {id}
        })
        res.json(todo)
    } catch (error) {

    }
})
 ```

 4.3.- enviamos la respuesta del error

 ```js
app.delete('/todos/:id', async (req, res) => {
    try{
        const { id } = req.params
        await Todo.destroy({
            where:{id}
        })
        res.status(204).end()
    }
    catch(error){
        res.status(400).json(error)
    }
}) 

```

### GET BY ID

5.- Creamos el metodo DELETE en nuestro archivo app.js

```js
app.get('/todos/:id', async (req, res) => {
    //aqui va todo lo que se ejecutara cuando reciba una peticion tipo GET BY ID
})
```

5.1.- colocamos un try/catch para la asincronia

```js
app.get('/todos/:id', async (req, res) => {
    try {
        
    } catch (error) {

    }
})
```
5.2.- destructuramos el req.params con la variable id para luego mandarla a llamar el
    metodo findByPk y enviamos la respuesta en json
```js

app.get('/todos/:id', async (req, res) => {
    try {
        const { id } = req.params
        const todo = await Todo.findByPk(id)
        res.json( todo )
    } catch (error) {

    }

})
 ```

 5.3.- enviamos la respuesta del error

 ```js
app.get('/todos/:id', async (req, res) => {
    try {
        const { id } = req.params
        const todo = await Todo.findByPk(id)
        res.json( todo )
    } catch (error) {
        res.status(400).json(error)
    }
})


```

## instalamos cors

```shell
npm i cors
```

## importamos cors en app.js

```js
import express from 'express';
import db from './utils/database.js';
import Todo from './models/todo.model.js';
import cors from 'cors'
```

1.- ejecutamos cors en app.js

```js
import express from 'express';
import db from './utils/database.js';
import Todo from './models/todo.model.js';
import cors from 'cors'

Todo;

const app = express()
const PORT = process.env.PORT ?? 8000
app.use(express.json())
app.use(cors())

db.authenticate()
    .then( () => console.log('conexion correcta'))
    .catch( (err) => console.log(err))

db.sync()
db.sync()
    .then(()=> console.log('base de datos sincronizada'))
    .catch((err)=> console.log(err))


app.get('/', (req, res)=>{
    res.send('OK')
})

app.get('/todos', async (req, res)=>{
    try {
        const todos = await Todo.findAll()
        res.json(todos);
    } catch (error) {
        res.status(400).json(error)
    }
})

app.post('/todos', async (req, res) => {
    try {
        const { body } = req
        const todo = await Todo.create(body)
        res.status(201).json(todo)
    } catch (error) {
        res.status(400).json(error)
    }
})

app.put('/todos/:id', async (req, res) => {
    try {
        const { id } = req.params
        const { body } = req
        const todo = await Todo.update(body, {
            where: {id}
        })
        res.json(todo)
    } catch (error) {
        res.status(400).json(error)
    }
})

app.delete('/todos/:id', async (req, res) => {
    try{
        const { id } = req.params
        await Todo.destroy({
            where:{id}
        })
        res.status(204).end()
    }
    catch(error){
        res.status(400).json(error)
    }
})

app.get('/todos/:id', async (req, res) => {
    try {
        const { id } = req.params
        const todo = await Todo.findByPk(id)
        res.json( todo )
    } catch (error) {
        res.status(400).json(error)
    }
})

app.listen(PORT, ()=>{
    console.log(`servidor corriendo en el puerto ${PORT}`)
})


```