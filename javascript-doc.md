## Importação e exportação de módulos e bibliotecas

### CommonJS

**const yourVar = require('yourLibsName');**

ex: const fs = require('fs');

<br>

**module.exports = yourFunction;**

<br>

### ESM

- Adicionar "type": "module", nas propriedades de package.json

**import yourVar from 'yourLibsName';**

ex: import fs from 'fs';

<br>

**export default function yourFunc() {...}   /  import yourFunc from './yourDir/yourFile.js';**

**export function yourFunc() {...}  /  import { yourFunc } from './yourDir/yourFile.js';**

<br><br>



## Objeto

**seuObjeto[chave] = valor**
- Para popular o objeto

## Métodos String

**.split()**
- Divide uma string em uma lista ordenada de substrings e retorna um array, a partir de um padrão para a divisão.

ex: const str = "Uma string de exemplo.";

const words = str.split(" ");

console.log(words);
// Expected output: Array ["Uma", "string", "de", "exemplo."]

<br>

**.toLowerCase()**
- Retorna o valor da string que foi chamada convertido para minúsculo.

<br>

**.replace()**
- Retorna uma nova string com algumas ou todas as correspondências de um padrão substituídas por um determinado caractere.

ex: const sentence = "Olá, amigo!";

console.log(sentence.replace('amigo', 'querido'));
// Expected output: Olá, querido!

<br>



<br><br>

## Métodos Array

**.forEach()**
- Executa uma dada função em cada elemento de um array.

ex: const array1 = ["a", "b", "c"];

array1.forEach((element) => {
    console.log(element);
});
// Expected output: "a" \n  "b"  \n  "c"

<br>

**.map()**
- Invoca a função callback passada por argumento para cada elemento do Array e devolve um novo Array como resultado.

ex: const array = [1, 2, 3, 4];

const map = array.map(x => x * 2);

console.log(map);
// Expected output: Array [2, 4, 6, 8]

<br>

**.filter()**
- Cria um novo array com todos os elementos que passaram no teste implementado pela função fornecida.

ex: const words = ["spray", "elite", "exuberant", "destruction", "present"];

const result = words.filter((word) => word.length > 6);

console.log(result);
// Expected output: Array ["exuberant", "destruction", "present"]

<br>

**.flatMap()**
- Mapeia cada elemento usando uma função de mapeamento e, em seguida, nivela (espalha os arrays internos no externo) o resultado em um novo array.

ex: const arr = [1, 2, 1];

const result = arr.flatMap(num => (num === 2 ? [2, 2] : 1));

console.log(result);
// Expected output: Array [1, 2, 2, 1]

<br>

**.sort()**
- Retorna o array ordenado.

ex: const array = ['cherries', 'apples', 'bananas']; 

arr.sort();

console.log(arr);
// Expected output: ['apples', 'bananas', 'cherries']

<br>

**.join()**
- Junta todos os elementos de um array em uma string.

ex: const elements = ["Fire", "Air", "Water"];

console.log(elements.join());
// Expected output: "Fire,Air,Water"

console.log(elements.join(""));
// Expected output: "FireAirWater"

<br><br>

## Object Methods

**Object.keys(obj)**
- Retorna um array de propriedades enumeráveis de um determinado objeto.

<br><br>

## Tratamento de exceções

**try...catch**
- Marca um bloco de declarações para testar e especifica uma resposta caso uma exceção seja lançada.

ex: try {
    try_statements
} catch (e) {
    handle_error
} 

<br><br>

## Programação Assíncrona

- Código que não bloqueia a execução do restante do programa.

### Promessas

- Promise é um objeto que representa a eventual conclusão ou falha de uma operação assíncrona e seu valor resultante e possui esses três estados: pending, fulfilled, rejected.

ex: 

const promise = new Promise((resolve, reject) => {
    // sua operação assíncrona
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve("Operação concluída com sucesso!");
        } else {
            reject("Algo deu errado...");
        }
    }, 2000);
});

promise.then(result => console.log(result))
       .catch(error => console.log(error))
       .finally(() => console.log("Fim da operação"));

<br>

ex com async...await:

async functions execute() {
    try {
        const data = await fetchData();
        console.log("Usuário:", data.user);
    } catch (error) {
        console.error("Erro:", error);
    }
}

execute();