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

<br>

**método POST**

```
// app.js
...

app.get...

app.post("/livros", (req, res) => {
    livros.push(req.body);
    res.status(201).send("Livro cadastrado com sucesso");
})

export default app;
```

<br>

**adicionar middleware**
- Executa a função em todas as requisições. Nas requisições os dados chegam como string. O método express.json() converte a string para o formato JSON.

```
//app.js
...
const app = express();
app.use(express.json());
...
```

<br>

**médoto PUT**

```
// app.js

app.get...

function buscaLivro(id) {
    return livros.findIndex(livro => {
        return livro.id === Number(id);
    });
}

app.get("/livros/:id", (req, res) => {
    const index = buscaLivro(req.params.id);
    res.status(200).json(livros[index]);
})

app.put("/livros/:id", (req, res) => {
    const index = buscaLivros(req.params.id);
    livros[index].titulo = req.body.titulo;
    res.status(200).json(livros);
})
```

<br>

**método DELETE**

```
// app.js

...

app.delete("/livros/:id", () => {
    const index = buscarLivros(req.params.id);
    livros.splice(index, 1);
    res.status(200).send("Livro removido com sucesso);
})
...
```

<br>

**adicionando bd e lib de conexão**

```
npm install mongodb

npm install mongoose
```

```
// src/config/dbConnect.js

import mongoose, { mongo } from "mongoose";

async function conectaNaDatabase() {
    mongoose.connect("your-connection-string");

    return mongoose.connection;
}

export default conectaNaDatabase;
```

```
// app.js

...
import conectaNaDatabase from "./config/dbConnect.js";

const conexao = await conectaNaDatabase();


conexao.on("error", (erro) => {
    console.error("erro de conexão", erro);
})

conexao.once("open", () => {
    console.log("Conexão com o banco feita com sucesso!");
})
...
```

<br>

**criando models e schemas**

```
// src/models/Livro.js

import mongoose from "mongoose";

const livroSchema = new mongoose.Schema({
    id: { type: mongoose.Schema.Types.ObjectId },
    titulo: { type: String, required: true },
    editora: { type: String },
    preco: { type: Number },
    paginas: { type: Number }
}, { versionKey: false });

const livro = mongoose.model("livros", livroSchema);

export default livro;

```

**acessando a coleção livros**

```
// app.js

...
import livro from "./models/Livro.js";
...

app.get("/livros", async(req, res) => {
    const listaLivros = await livro.find({});
    res.status(200).json(listaLivros);
})
```

**coloque a connection string num .env**

`npm install dotenv`

```
// .env

DB_CONNECTION_STRING=your-connection-string
```

```
//.gitignore
...
.env
```

```
// server.js
import "dotenv/config";
...
```

```
//dbConnect.js

...
    mongoose.connect(process.env.DB_CONNECTION_STRING);
...
```

**criando controller para livro**
- Controller faz o link entre a requisição e o que vai acontecer com essa requisição.

```
// src/controller/livroController.js

import livro from "../models/Livro.js";

class LivroController {

    static async listarLivros(req, res) {
        try {
        const listaLivros = await livro.find({});
        res.status(200).json(listaLivros);
        } catch (erro) {
            res.status(500).json({ message: `${erro.message}, falha na requisição` };)
        }
    }

};

export default LivroController;
```

```
// src/routes/livrosRoutes.js

import express from "express";
import LivroController from "../controllers/livroController.js";

const routes = express.Router();

routes.get("/livros", LivroController.listarLivros);
routes.get("/livros/:id", LivroController.listarLivrosPorId);
routes.put("/livros", LivroController.cadastrarLivro);
routes.update("/livros/:id", LivroController.atualizarLivro);
routes.delete("/livros/:id", LivroController.deletarLivro)
```
<br>
- assim, pode-se deletar o método .get do app.js e fazer o mesmo para os outros métodos
<br>

```
// livroController.js

...
    static async cadastrarLivro(req, res) {
        try {
            const novoLivro = await livro.create(req.body);
            res.status(201).send({ message: "criado com sucesso!", livro: novoLivro });
        } catch (erro) {
            res.status(500).json({ message: `${erro.message} - falha ao cadastrar livro` });
        }
    }

    static async listarLivroPorId (req, res) {
        try {
            const id = req.params.id;
            const livroEncontrado = await livro.findById(id);
            res.status(200).json(livroEncontrado);
        } catch (erro) {
            res.status(500).json({ message: `${erro.message}, falha na requisição do livro` };)
        }
    }

    static async atualizarLivro (req, res) {
        try {
            const id = req.params.id;
            await livro.findByIdAndUpdate(id, req.body);
            res.status(200).json({ message: "livro atualizado!" });
        } catch (erro) {
            res.status(500).json({ message: `${erro.message}, falha na atualização` };)
        }
    }

    static async deletarLivro (req, res) {
        try {
            const id = req.params.id;
            await livro.findByIdAndDelete(id);
            res.status(200).json({ message: "livro deletado!" });
        } catch (erro) {
            res.status(500).json({ message: `${erro.message}, falha ao deletar` };)
        }
    }
...
```

```
// routes/index.js

import express from "express";
import livros from "./livrosRoutes.js";

const routes = (app) => {
    app.route("/").get((req, res) => res.status(200).send("Curso de Node.js));

    app.use(express.json(), livros);
};

export default routes;
```

- retirar a importação do model Livro.js e importar as rotas e o app.use(express.json());

```
// app.js

...
import routes from "./routes/index.js";
...

const app = express();
routes(app);
``` 

<br>

**criando autores**

```
// Autor.js

import mongoose from "mongoose";

const autorSchema = new mongoose.Schema({
    id: { type.mongoose.Schema.Types.ObjectId },
    nome: { type: String, requirede:true },
    nacionalidade: { type: String }
}, { versionKey: false });

const autor = mongoose.model("autores", autorSchema);

export { autor, autorSchema };
```

- o autoresRoutes.js e o autorController.js é igual ao dos livros mudando somente as relações com autor.

```
// Autor.js
import { autorSchema } from "./Autor.js";
...
    autor: autorSchema
...
```

```
// livroController.js
import { autor } from "../models/Autor.js";

...
    const novoLivro = req.body;

    try {
        const autorEncontrado = await autor.findById(novoLivro.autor);
        const livroCompleto = { ...novoLivro, autor: { ...autorEncontrado._doc }};
        const livroCriado = await livro.create(livroCompleto);
    }
...
```

<br>

**buscas por parâmetro**

```
// livroController.js

...
    static async listarLivrosPorEditora (req, res) {
        const editora = req.query.editora;
        try {
            const livrosPorEditora = await livro.find({ editora: editora });
            res.status(200).json(livrosPorEditora);
        } catch (erro) {
            res.status(500).json({ message: `${erro.message} - falha na busca` });
        }
    }
...
```

```
// livrosRoutes.js
- rotas precisam ter ordem de complexidade do menos para o mais: /livros, /livros/busca, /livros:id...

...
routes.get("/livros/busca", LivroController.listarLivrosPorEditora);
...
```

**tratando erros de busca por id**

```
// autoresController.js

...
    static listarAutorPOrId... {
        try {
            ...
            if (autorResultado !== null) {
                res.status(200).send(autorResultado)
            } else {
                res.status(400).send({ message: "Id do Autor não localizado." });
            }
        } catch (erro) {
            if (erro instaceof mongoose.Error.CastError) {
                res.status(404).send({ message: "Um ou mais dados fornecidos estão incorrestos." });
            } else {
                res.status(400).send({ message: "Erro interno de servidor." });
            }
        }
    }
...
```

<br>

**middlewares do express**

- são funções que intercptam alguma ação.

```
// app.js

...
app.use(express);
routes(app);

app.use((erro, req, res, next) => {
    if (erro instaceof mongoose.Error.CastError) {
        res.status(404).send({ message: "Um ou mais dados fornecidos estão incorrestos." });
    } else {
        res.status(400).send({ message: "Erro interno de servidor." });
    }
});
...
```

```
// autoresController.js

...
    static listarAutorPorId = async (req, res, next)...
        ...
        catch (erro) {
            next(erro);
        }
...
```

- fazer o mesmo para os outros catches
- coloque o middleware numa pasta 

```
// src/middlewares/manipuladorDeErros.js

import mongoose from "mongoose";

function manipuladorDeErros (erro, req, res, next) {
    if (erro instaceof mongoose.Error.CastError) {
        res.status(404).send({ message: "Um ou mais dados fornecidos estão incorrestos." });
    } else {
        res.status(400).send({ message: "Erro interno de servidor." });
    }
}

export default manipuladorDeErros;
```

```
// app.js

...
app.use(manipuladorDeErros);
...
```

<br>

**erros de validação**

```
// src/middlewares/manipuladorDeErros.js

function manipuladorDeErros (erro, req, res, next) {
    if (erro instaceof mongoose.Error.CastError) {
        res.status(404).send({ message: "Um ou mais dados fornecidos estão incorrestos." });
    } else if (erro instanceof mongoose.Error.ValidationError) {
        const mensagensErro = Object.values(erro.erros)
            .map(erro => erro.message)
            .join("; ");

        res.status(400).send({ message: `Os seguintes erros foram encontrados: ${ mesangenErro }` });
    } else {
        res.status(400).send({ message: "Erro interno de servidor." });
    }
}

export default manipuladorDeErros;
```

```
// Autor.js

...
    const autorSchema = ...
    nome: {
        type: String,
        required: [true, "O nome do(a) autor(a) é obrigatório."]
    }
...
```

<br>

**refatorando o manipulador de erros**

```
// src/erros/ErroBase.js

class ErroBase extends Error {
    constructor(mensagem = "Erro interno do servidor.", status = 500) {
        super();

        this.message = mensagem;
        this.status = status;
    }

    enviarReposta (res) {
        res.status(this.status).send({
            mensagem: this.message,
            status: this.status
        });
    }
}

export default ErroBase;
```

```
//manipuladorDeErros.js

...
import ErroBase from "../erros/ErroBase.js";
...

function manipualdorDeErros (...
    ...
    else {
        new ErroBase().enviarResposta(res);
    }
...
```

```
// src/erros/RequisicaoIncorreta.js

import ErroBase from "./ErroBase.js";

class RequisicaoIncorreta extends ErroBase {
    constructor(mensagem = "Um ou mais dados fornecidos estão incorretos!") {
        super(mensagem, 400);
    }
}

export default RequisicaoIncorreta;
```

```
//manipuladorDeErros.js

...
import ErroBase from "../erros/ErroBase.js";
import RequisicaoIncorreta from "../erros/RequisicaoIncorreta.js";
...

function manipuladorDeErros (...
    ...
    if (erro instanceof mongoose.Error.CastError) {
        new RequisicaoIncorreta().enviarResposta(res);
    } else if ...
...
```

```
// src/erros/ErroValidacao.js

import RequisicaoIncorreta from "./RequisicaoIncorreta.js";

class ErroValidacao extends RequisicaoIncorreta {
    constructor(erro) {
        const mensagensErro = Object.values(erro.errors)
            .map(erro => erro.message)
            .join("; ");

        super(`Os seguintes erros foram encontrados: ${mensagensErro}`);
    }
}

export default ErroValidacao;
```

```
//manipuladorDeErros.js

...
import ErroValidacao from "../erros/ErroValidacao.js";
...

function manipualdorDeErros (...
    ...
    else if (erro instanceof mongoose.Error.ValidationError) {
        new ErroValidacao(erro).enviarResposta(res);
    } else ...
...
```

<br><br>

**tratando página 404**

**ATENÇÃO: A ORDEM DE IMPORTAÇÃO DOS MANIPULADORES IMPORTA PARA O FUNCIONAMENTO CORRETO, ASSIM COMO O POSICIONAMENTO DOS MIDDLEWARES NO APP.JS**
- deve se colocar primeiro a importação dos middlewares e depois das rotas
- acionar o .use dos middlewares depois do routes(app)

```
// app.js
import manipulador404 from "./middlewares/manipulador404.js";
...
route(app);

app.use(manipulador404);
...
```

```
// src/middlewares/manipulador404.js
import NaoEncontado from "../erros/NaoEncontrado.js";

function manipulador404(req, res, next) {
    const erro404 = new NaoEncontrado();
    next(erro404);
}

export default manipulador404;
```

```
// src/erros/NaoEncontrado.js
import ErroBase from "./ErroBase.js"

class NaoEncontrado extends ErroBase {
    constructor(mensagem = "Página não encontrada.") {
        super(mensagem, 404);
    }
 }

export default NaoEncontrado;
```

```
// src/middlewares/manipuladorDeErros.js

function manipulador...
    else if (erro instanceof NaoEncontrado) {
        erro.enviarReposta(res);
    }
```
```
// src/controllers/autoresController.js
import NaoEncontrado from "../erros/NaoEncontrado.js";
...
    static listarAutorPorId...
        else {
         next(new NaoEncontado("Id do Autor não localizado."));
        }
...
```

<br><br>

**validações do mongoose**

```
// Livro.js

...
    numeroPaginas: {
        type: Number,
        min: 10,
        max: 5000
    }
...
```

