## Terminal

**process.argv**
- Retorna um array contendo os argumentos da linha de comando

<br>

**fs.readFile**
- Lê o conteúdo de um arquivo assincronamente.

ex: fs.readFile('your/Files/Path', 'utf-8', (err, data) => {
        if (err) throw err;
        console.log(data);
});

<br><br>

## Commander

- Pacote para descrever suas CLIs.

**npm install commander**

```

