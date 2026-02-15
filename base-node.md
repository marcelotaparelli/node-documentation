# Base

<br>

### V8 + Libuv + Bindings + APIs internas = Runtime
O **Runtime**, é um **conjunto de ferramentas, bibliotecas e mecanismos que permitem a execução de um programa** escrito em uma determinada linguagem.

<br>

**V8**: é **o motor JavaScript** criado pelo Google e escrito em C++, usado no Chrome e no Node.js. É responsável por **transformar texto em código de máquina nativo e executar o código JS**.

Passo a passo do funcionamento do v8:

**1. Parsing**
- O código JS é lido e transformado em tokens (palavras-chave, variáveis, operadores).
- Esses tokens são organizados em uma AST (Abstract Syntax Tree) que representa a estrutura lógica do programa.

**2. Ignition (Interpretador interno)**
- A AST é convertida em bytecode - uma linguagem intermediária mais próxima da máquina
- o interpretador Ignition começa a executar esse bytecode imediatamente

**3. Profiler (Analisador de uso)**
- Enquanto o código roda, o V8 observa quais funções e trechos são usados com frequência. Ele identifica "hot code", ou seja, partes que valem a pena otimizar (esse processo se chama *profiling*).

**4. TurboFan (Compilador JIT - Just-In-Time)**
- O TurboFan pega o bytecode das partes mais usadas e compila para código de máquina nativo e aplica otimizações avançadas.

**5. Execução final**
- O processador executa diretamente o código de máquina nativo gerado pelo TurboFan.
- O ciclo interpretar -> otimizar -> executar garante que o JS rode com desempenho próximo ao de linguagens compiladas.

<br><br><br>

## Libs externas

**npm install yourPackage**
- Instala pacotes externos

**npm audit**
- Roda análise automática de vulnerabilidades conhecidas

**nvm** 
- Node version manager
ex: nvm use 16

## Checklist para avaliar pacotes npm em produção

**Popularidade e manutenção**
- Verifique **downloads semanais** no npm (milhares = mais confiável).
- Veja a **última atualização** (se está ativo ou abandonado).
- Analise o **histórico de versões** (lançamentos frequentes indicam manutenção).

**Autor e comunidade**
- Confira **quem mantém** (empresa, dev conhecido, comunidade ativa).
- Olhe **issues e PRs** no GitHub (se são respondidos e fechados).
- Veja **estrelas e forks** no GitHub (indicador de adoção).

**Segurança**
- Rode `npm audit` para detectar vulnerabilidades.
- Analise **dependências** (quanto menos, melhor).
- Cuidado com **typosquatting** (nomes parecidos com pacotes famosos).

**Qualidade do código**
- Leia a **documentação** (clara e atualizada).
- Verifique se há **testes automatizados**.
- Veja se suporta **TypeScript** (tipos oficiais ou `@types`).

**Licença**
- Confirme que é **open source** e permite uso comercial.
- Evite pacotes sem licença ou com licenças obscuras.

**Riscos comuns**
- Pacotes **abandonados** → vulneráveis.
- Pacotes **novos demais** → sem validação de mercado.
- Pacotes com **dependências suspeitas** → risco de malware.





