## Criando uma API Rest com Express e MongoDB

### Servidor básico

**npm init -y**
- Para criar o package.json.

<br>

**adicione `"type": "module",` no package.json**

<br>

**crie um server.js**

```
import http from 'http';

const PORT = 3000;

const server = http.createServer((req, res) => {
    res.writeHead(200, {"Content-Type": "text/plain"});
    res.end("API com Node.js");
});

server.listen(PORT, () => {
    console.log("servidor escutando!");
});
```

<br>

**adicione as rotas**

```
const rotas = {
    "/": "Curso de Node.js",
    "/livros": "Entrei na rota livros",
    "/autores": "Entrei na rota autores"
}

const server = ...
    ...
    res.end(rotas[req.url]);
    ...
```

<br><br>

### Servidor com Express

**crie uma pasta src**
- Adicione um aquivo app.js

```
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.status(200).send("Curso de Node.js");
})

export default app;
```

<br>

**no sever.js**

```
import app from "./src/app.js";

const PORT = 3000;

app.listen(PORT, () => {
    console.log("Servidor escutando!");
})
```
<br>

**criando dados fictícios**

```
// app.js

...

const app = express();

const livros = [
    {
        id: 1,
        titulo: "O Senhor dos Anéis"
    },
    {
        id: 2,
        titulo: "O Hobbit"
    }
];

...

app.get("/livros", (req, res) => {
    res.status(200).json(livros);
});

```