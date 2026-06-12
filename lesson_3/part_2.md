# Parte 2

O conhecimento acumulado até aqui cobre declaração de funções, passagem como argumento e retorno de outras funções. Há, porém, uma pergunta que permaneceu em aberto: quando uma função acessa uma variável, de onde ela a busca?

A resposta parece imediata. Mas considere: e se a variável estiver fora da função? E se estiver em outra função que já terminou de executar? E se houver duas variáveis com o mesmo nome em lugares diferentes — qual prevalece?

Essas perguntas não são teóricas. Elas explicam comportamentos recorrentes no desenvolvimento com JavaScript — bugs difíceis de rastrear, código que funciona de maneira inesperada e padrões poderosos que só fazem sentido quando o mecanismo subjacente está compreendido.

O mecanismo tem nome: escopo. E o fenômeno mais importante que emerge dele tem outro nome: closure.

---

## Bloco 1 — Escopo Léxico

### A intuição

Imagine um prédio de escritórios com salas dentro de salas. Na sala mais interna, há um estagiário que precisa de uma folha de papel. Ele procura primeiro na própria mesa. Não encontra. Abre a porta e procura na sala de fora. Não encontra. Vai ao corredor. Encontra lá.

Agora a pergunta: o estagiário poderia ter ido direto ao andar de cima, pulado a sala do meio e procurado em qualquer lugar do prédio em ordem aleatória?

Não. A busca segue uma ordem determinada pela posição física de cada sala: de dentro para fora, em sequência, sem pular.

JavaScript busca variáveis da mesma forma.

---

### A definição

**Escopo** é a região do código onde um identificador (o nome de uma variável, de uma função) é visível e pode ser acessado.

**Escopo léxico** significa que esse escopo é determinado pela posição do código no texto do programa — não pelo momento em que o código executa, não por quem chamou a função, mas por onde a função foi escrita.

*"Léxico"* vem de *lexis* — palavra, texto. O escopo está no texto, não na execução.

---

### Como a busca funciona

Quando JavaScript encontra um identificador, ele segue esta ordem de busca:

1. Procura no escopo atual (o bloco ou função onde o código está).
2. Se não encontrar, sobe para o escopo que contém este — o escopo externo imediato.
3. Continua subindo até chegar ao escopo global.
4. Se não encontrar nem lá, lança `ReferenceError`.

Essa sequência de escopos aninhados tem nome: cadeia de escopos (*scope chain*).

```javascript
const message = 'outside';

function external() {
  const message = 'from the outside';

  function internal() {
    console.log(message);
  };

  internal();
};

external();
```

Quando `internal` executa e encontra `message`, ela procura primeiro dentro de si mesma. Não encontra nenhuma declaração de `message` no próprio corpo. Sobe para `external`. Encontra `const message = 'from the outside'`. Para a busca. Imprime `'from the outside'`.

A variável `message` do escopo global nunca é consultada, porque a busca terminou antes.

Se `external` também não tivesse `message`, a busca chegaria ao global e encontraria `"outside"`.

Se não houvesse `message` em lugar nenhum, o resultado seria `ReferenceError: message is not defined`.

---

### Ponto central

O escopo de `internal` foi determinado no momento em que `internal` foi escrita dentro de `external`. Não importa de onde `internal` é chamada. Não importa quando é chamada. O escopo léxico é fixado pela posição no texto.

---

## Bloco 2 — Escopo Global, de Função e de Bloco

### A intuição

O prédio da analogia anterior tem três tipos de sala.

A primeira é o saguão de entrada — acessível a todo mundo, de qualquer ponto do prédio. Qualquer coisa deixada ali pode ser vista e pegada por qualquer pessoa.

A segunda é uma sala privativa — só quem está dentro dela enxerga o que há lá. Quem está no corredor não tem acesso.

A terceira é uma cabine temporária dentro da sala privativa — existe enquanto alguém está usando, depois some. E só quem está na cabine enxerga o que está nela.

Esses três ambientes mapeiam diretamente os três tipos de escopo em JavaScript.

---

### Escopo global

Qualquer declaração feita fora de toda função e fora de todo bloco existe no escopo global. É visível em qualquer ponto do programa.

```javascript
const language = 'portuguese';

function greet() {
  console.log(language);
};

greet(); // 'portuguese'
```

`language` está no escopo global. `greet` a encontra ao subir a cadeia de escopos.

O escopo global é o último nível da cadeia. Se a busca chegar aqui e não encontrar o identificador, JavaScript lança `ReferenceError`.

---

### Escopo de função

Toda declaração feita dentro de uma função existe apenas naquele escopo. Nada de fora pode acessá-la.

```javascript
function calculate() {
  const result = 42;
  console.log(result); // 42
};

calculate();
console.log(result); // ReferenceError: result is not defined
```

`result` existe dentro de `calculate` e em lugar nenhum mais. A linha depois da chamada tenta acessá-la de fora — e o JavaScript não a encontra em lugar nenhum na cadeia de escopos.

---

### Escopo de bloco

Um bloco é qualquer par de chaves `{}` — o corpo de um `if`, de um `for`, de um `while`, ou até chaves isoladas.

É neste ponto que `var`, `let` e `const` se comportam de maneira fundamentalmente diferente.

`let` e `const` respeitam o escopo de bloco. Uma variável declarada com `let` ou `const` dentro de um bloco existe apenas dentro daquele bloco.

```javascript
if (true) {
  let status = 'active';
  console.log(status); // 'active'
};

console.log(status); // ReferenceError: status is not defined
```

`status` foi declarada dentro do bloco do `if`. Fora desse bloco, ela não existe.

`var` ignora o escopo de bloco. Uma variável declarada com `var` dentro de um bloco vaza para o escopo de função mais próximo — ou para o escopo global, se não houver função.

```javascript
if (true) {
  var status = 'active';
  console.log(status); // 'active'
};

console.log(status); // 'active'
```

Mesmo tendo sido declarada dentro do bloco do `if`, `status` com `var` está disponível fora dele. Ela se comporta como se tivesse sido declarada no topo do escopo que a contém.

Esse comportamento de `var` é uma fonte histórica de bugs difíceis de rastrear. O código parece conter a variável dentro do bloco, mas ela não está contida. Por isso o curso adota `let` e `const` como padrão exclusivo — elas se comportam como o programador intuitivamente espera.

---

### A consequência prática

A escolha entre `var`, `let` e `const` não é apenas estética. Ela determina onde a variável existe. Com `let` e `const`, o código diz exatamente o que faz: a variável vive no bloco onde foi declarada e em lugar nenhum mais.

---

## Bloco 3 — Hoisting

### A intuição

Imagine a organização de uma festa em que é necessário saber quais convidados vão aparecer antes de eles chegarem — para preparar os crachás com antecedência. Uma lista com os nomes de todos os convidados confirmados é elaborada antes de a festa começar. Quando cada um chega de fato, o crachá já está pronto.

JavaScript faz algo semelhante antes de executar o código: percorre o escopo e registra as declarações antes de executar qualquer linha. Esse processo tem nome: **hoisting**.

*"Hoist"* em inglês significa içar, elevar. A ideia é que as declarações são conceitualmente "elevadas" para o topo do escopo antes da execução.

---

### O que é hoisted

Hoisting não move código fisicamente. O que acontece é que o motor JavaScript processa as declarações em duas passagens: primeiro registra o que foi declarado, depois executa linha a linha. O efeito prático é que certas declarações parecem estar disponíveis antes de onde foram escritas.

Existem três comportamentos distintos, dependendo do que está sendo declarado.

---

### Declarações de função são totalmente hoisted

Uma função declarada com `function` está completamente disponível antes da linha onde foi escrita.

```javascript
greet();

function greet() {
  console.log('Hello!');
};
```

Isso funciona. JavaScript registrou `greet` com seu corpo completo antes de executar qualquer linha. A chamada na primeira linha encontra a função já disponível.

Esse é o comportamento observado anteriormente: declarações de função podem ser chamadas antes de sua posição no texto. Funções expressas em variáveis (`const f = function(){}` ou `const f = () => {}`) não têm esse comportamento — apenas a variável é hoisted, não o valor atribuído a ela.

---

### Declarações de `var` são hoisted, mas com valor `undefined`

```javascript
console.log(name); // undefined
var name = 'Ana';
console.log(name); // 'Ana'
```

O que acontece: JavaScript registra a existência de `name` antes de executar. Mas o valor `"Ana"` só é atribuído quando a linha de declaração é executada. Antes disso, `name` existe — mas seu valor é `undefined`.

Essa é uma armadilha clássica. O código não lança erro — ele silenciosamente produz `undefined`, que pode se propagar pelo programa de maneiras difíceis de rastrear.

---

### Declarações de `let` e `const` são hoisted, mas não inicializadas

Este é o comportamento mais importante de entender, e ele tem nome próprio — que será apresentado no próximo bloco.

```javascript
console.log(city); // ReferenceError

let city = 'São Paulo';
```

`let` e `const` são registradas pelo JavaScript antes da execução — elas existem no escopo desde o início. Mas ficam em um estado especial: existem, mas não podem ser acessadas. Qualquer tentativa de acesso antes da linha de declaração lança `ReferenceError`.

Por que o comportamento é diferente de `var`? Porque `var` foi projetado com hoisting inicializando para `undefined` — um design que causa confusão. `let` e `const` foram projetados para tornar esse problema visível: o acesso antes da declaração produz erro imediatamente, em vez de retornar `undefined` de forma silenciosa.

---

### Ponto central

Hoisting não é um recurso para usar intencionalmente — é um mecanismo do motor que é necessário conhecer para entender por que certas coisas funcionam (chamada de função antes da declaração) e por que certas coisas falham (acesso a `let`/`const` antes da declaração).

---

## Bloco 4 — Temporal Dead Zone

### A intuição

O mecanismo acima foi descrito. Agora ele ganha um nome formal.

Imagine uma contratação: o crachá já foi impresso e está na gaveta da recepção — mas ainda não pode ser utilizado porque a data de início é amanhã. A pessoa existe no sistema. O acesso está registrado. Mas qualquer tentativa de entrada antes do dia certo é bloqueada.

Esse intervalo — entre o momento em que o registro foi feito e o momento em que o acesso de fato se torna possível — é a **Temporal Dead Zone**.

---

### A definição

A **Temporal Dead Zone** (TDZ) é o intervalo que começa no início do escopo onde uma variável `let` ou `const` foi declarada e termina na linha exata dessa declaração.

Durante esse intervalo, a variável existe no escopo — foi registrada pelo hoisting — mas está em estado não inicializado. Qualquer acesso durante a TDZ lança `ReferenceError`.

```javascript
function execute() {
  console.log(points); // ReferenceError

  let points = 100;

  console.log(points); // 100
};

execute();
```

A TDZ de `points` começa no momento em que o escopo de `execute` é criado — antes de qualquer linha executar. Termina exatamente na linha `let points = 100;`. Entre esses dois pontos, qualquer acesso a `points` é bloqueado.

---

### Por que tem nome próprio

O nome existe porque o comportamento é contra-intuitivo no primeiro contato. A variável foi registrada pelo hoisting — então ela existe. Mas não pode ser acessada. Isso parece contraditório até que se compreenda que existir no escopo e estar inicializada são duas coisas diferentes.

O nome Temporal Dead Zone nomeia exatamente essa distinção: é uma zona morta no tempo — um período em que a variável está presente mas inacessível.

---

### A diferença prática

Com `var`, não existe TDZ. A variável é hoisted e imediatamente inicializada com `undefined`. O acesso antes da declaração é possível — o resultado é `undefined`, sem erro.

Com `let` e `const`, a variável é hoisted mas não inicializada. O acesso antes da declaração não retorna `undefined` de forma silenciosa — lança `ReferenceError` imediatamente, tornando o problema visível.

Essa é a razão pela qual `let` e `const` são superiores para escrever código previsível: erros aparecem na hora certa, no lugar certo.

---

## Bloco 5 — Closure

### A intuição

Quando uma função termina de executar, seu escopo local desaparece. As variáveis declaradas dentro dela somem.

Mas existe uma situação em que isso não acontece.

Imagine uma mochila. Quando uma função é escrita dentro de outra função, a função interna pega uma mochila e coloca dentro dela todas as variáveis do escopo externo que ela referencia. Quando a função externa termina e seu escopo seria destruído, a mochila ainda existe — porque a função interna a carrega consigo.

Enquanto a função interna existir, as variáveis dentro da mochila sobrevivem.

---

### A definição

Uma **closure** é uma função que mantém acesso ao escopo léxico em que foi declarada, mesmo após esse escopo ter terminado a execução.

Por que isso funciona: o motor JavaScript detecta que a função interna referencia variáveis do escopo externo. Em vez de destruir esse ambiente quando a função externa termina, ele o preserva — vinculado à função interna. O ambiente sobrevive enquanto a função interna sobreviver.

---

### Quando uma closure emerge

Toda função aninhada que referencia variáveis do escopo externo é, formalmente, uma closure. Nenhuma ação especial é necessária para criá-la — ela emerge naturalmente do escopo léxico.

```javascript
function createCounter() {
  let count = 0;

  function increment() {
    count++;
    console.log(count);
  };

  return increment;
};

const counter = createCounter();

counter(); // 1
counter(); // 2
counter(); // 3
```

Percorra o que acontece linha a linha:

`createCounter` é chamada. Um escopo é criado. `count` é declarada com valor `0`. `increment` é declarada dentro desse escopo — e, ao ser declarada, ela captura o ambiente: ela tem acesso a `count`.

Mas `increment` foi retornada e armazenada em `counter`. Ela ainda existe. E ela referencia `count`. Por isso o motor preserva o ambiente — `count` continua viva, vinculada a `increment`.

Cada chamada a `counter()` acessa e modifica a mesma contagem. Ela persiste entre chamadas porque está no ambiente capturado pela closure.

---

### Referência ao ambiente, não ao valor

A closure não é uma cópia do valor de `count` no momento em que `increment` foi criada. É uma referência viva ao ambiente onde `count` vive. Por isso as chamadas sucessivas acumulam — cada uma modifica a mesma variável, e a modificação persiste.

Se a closure capturasse apenas o valor, cada chamada partiria de `0`. Ela captura o ambiente — por isso parte de onde a última chamada parou.

---

### Isolamento de ambientes entre instâncias

```javascript
const counterA = createCounter();
const counterB = createCounter();

counterA(); // 1
counterA(); // 2
counterB(); // 1
```

Cada chamada a `createCounter` cria um escopo novo — um ambiente novo, com sua própria `count`. `counterA` e `counterB` são closures distintas, cada uma com seu próprio ambiente capturado. Não compartilham estado.
