## Importação de módulos e bibliotecas

**const yourVar = require('yourLibsName');**

ex: const fs = require('fs');

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

## Métodos Array

**.forEach()**
- Executa uma dada função em cada elemento de um array.

ex: const array1 = ["a", "b", "c"];

array1.forEach((element) => {
    console.log(element);
});
// Expected output: "a" \n  "b"  \n  "c"