## Criando uma API Rest com Express e MongoDB

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
    "/": "Curso de Node.js"
}

const server = ...
    ...
    res.end(rotas[req.url]);
    ...
```
