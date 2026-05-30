# Parte 2

## Bloco 1 — Variáveis

### O problema

Imagine uma receita que exige anotar quantos ovos já foram colocados na tigela. O registro no papel seria: *"ovos usados: 3"*. Depois de colocar mais um, rasura-se e escreve-se *"ovos usados: 4"*.

Esse papel com o valor anotado é o que um programa chama de variável: um nome que aponta para um valor guardado na memória do computador, de forma que seja possível consultá-lo e, quando necessário, alterá-lo.

Em JavaScript, toda variável precisa ser declarada antes de ser usada. Declarar significa dizer ao runtime: *"reserve um espaço com esse nome"*. A linguagem oferece três palavras para fazer essa declaração: `var`, `let` e `const`.

---

### `var` — a declaração original

`var` é a forma mais antiga de declarar variáveis em JavaScript. Ela existe desde a criação da linguagem e aparece em código escrito antes de 2015. O motivo de estudá-la não é para usá-la, mas para que esse código possa ser lido sem estranheza.

O problema central de `var` é o seu escopo. Escopo é a região do programa onde uma variável existe e pode ser acessada. `var` tem escopo próprio, e isso gerava comportamentos contraintuitivos que causavam problemas difíceis de rastrear.

Em código novo, `var` está proibida. Não será utilizada em nenhum momento deste curso.

---

### `let` — reatribuição com escopo seguro

`let` foi introduzida em 2015 junto com uma revisão importante da linguagem. Ela resolve o problema de escopo do `var`: uma variável declarada com `let` existe apenas no bloco onde foi criada. Um bloco é qualquer região delimitada por chaves `{ }`.

`let` é a declaração adequada quando o valor da variável precisar ser substituído ao longo do programa.

Exemplo concreto: a contagem de tarefas concluídas começa em zero e aumenta a cada tarefa marcada como feita. O valor muda, então `let` é a declaração adequada.

```javascript
let completed = 0;

completed = 1;

completed = 2;
```

**Finalidade.** `let` é uma declaração que reserva um espaço nomeado na memória e permite que o valor armazenado nesse espaço seja substituído posteriormente.

**Motivação.** Certos valores mudam conforme o programa avança — uma contagem que cresce, um estado que alterna. Para esses casos, a declaração precisa permitir reatribuição.

**Decisão.** `let` em vez de `var` porque o escopo de bloco é previsível e seguro; `let` em vez de `const` porque haverá reatribuição.

**Localização.** A variável `completed` existe a partir da linha de declaração até o fim do bloco que a contém. Qualquer linha dentro desse bloco pode lê-la ou substituí-la.

**Contrafactual.** Omitir a declaração e tentar usar o nome diretamente faz o Node.js interromper a execução com `ReferenceError: completed is not defined`. Usar `var` no lugar produz comportamento funcionalmente igual neste exemplo simples, mas o escopo se torna imprevisível em estruturas mais complexas, o que será visto nas próximas aulas.

---

### `const` — o padrão para valores que não mudam

`const` também foi introduzida em 2015 e tem o mesmo escopo de bloco que `let`. A diferença é que uma variável declarada com `const` não pode ser reatribuída depois da declaração.

`const` é o padrão do curso. Toda declaração começa com `const`. A mudança para `let` ocorre apenas quando, ao analisar o problema, se conclui que o valor precisará ser substituído.

Exemplo concreto: o nome do projeto é fixo. Esse é um candidato natural a `const`.

```javascript
const project = 'Tasks';
```

**Finalidade.** `const` é uma declaração que reserva um espaço nomeado na memória e impede que esse nome seja associado a outro valor após a declaração.

**Motivação.** A maioria dos valores em um programa bem escrito não precisa mudar. Usar `const` como padrão força a reflexão consciente sobre quando um valor realmente muda — esse pensamento é, em si, uma prática de qualidade.

**Decisão.** `const` em vez de `let` porque não haverá reatribuição. `const` não significa que o valor é imutável em todos os sentidos — esse detalhe será visto mais à frente no curso. Por ora, o que importa é: `const` proíbe substituição do nome por outro valor.

**Localização.** `project` existe a partir da linha de declaração até o fim do bloco. Qualquer tentativa de fazer `project = 'Other Things';` depois resulta em `TypeError: Assignment to constant variable`.

**Contrafactual.** Usar `let` onde `const` seria suficiente não impede o funcionamento do código — mas elimina a proteção contra reatribuição acidental e envia um sinal incorreto para quem lê o código: *"esse valor pode mudar"*, quando na verdade não muda.

---

## Bloco 2 — Tipos Primitivos

### O problema

Voltando ao papel da receita: os valores *"ovos usados: 3"* e *"ingrediente: farinha"* são fundamentalmente diferentes. Um é um número com o qual se pode fazer contas; o outro é um texto que se lê. Somar *"farinha"* com 3 não faz sentido.

Programas lidam com o mesmo problema. Um valor que representa uma quantidade se comporta de forma diferente de um valor que representa um nome, que se comporta de forma diferente de um valor que representa uma resposta de sim ou não. O JavaScript precisa saber com que tipo de valor está lidando para saber o que pode ser feito com ele.

Esses tipos fundamentais — os blocos mais básicos de dado que a linguagem reconhece — são chamados de **tipos primitivos**.

---

### Os sete tipos primitivos do JavaScript

O JavaScript tem exatamente sete tipos primitivos. Cada um existe por uma razão distinta.

---

#### `string` — texto

Qualquer sequência de caracteres: uma letra, uma palavra, uma frase, um parágrafo inteiro. Pode ser delimitada por aspas simples, aspas duplas ou crase. A crase tem comportamento especial que será visto no Bloco 4.

```javascript
const name = 'Alex';

const greeting = 'Hello!';
```

**Aplicação.** Nomes, títulos, descrições, endereços de e-mail — qualquer dado que o usuário digita ou que o programa exibe como texto.

**Justificativa.** Sem um tipo dedicado a texto, o programa não teria como representar a diferença entre o número `42` e o texto `'42'` — e essa diferença importa, como será visto no Bloco 5.

---

#### `number` — número

Qualquer valor numérico: inteiro ou decimal. O JavaScript não separa os dois em tipos distintos — ambos são `number`.

```javascript
const amount = 5;

const price = 19.90;
```

**Aplicação.** Contagens, preços, índices, resultados de cálculos, qualquer grandeza mensurável.

**Justificativa.** Números precisam de operações matemáticas — soma, subtração, comparação de magnitude. Texto não. Ter tipos separados permite que a linguagem saiba quais operações fazem sentido.

---

#### `boolean` — verdadeiro ou falso

Exatamente dois valores possíveis: `true` e `false`. Nada mais.

```javascript
const completed = true;

const logged = false;
```

**Aplicação.** Estados binários — algo está ativo ou inativo, marcado ou não marcado, permitido ou proibido.

**Justificativa.** Decisões em programas são sempre binárias no nível mais fundamental. Esse usuário está autenticado? Sim ou não. Essa tarefa está concluída? Sim ou não. O tipo `boolean` é o tipo das decisões.

---

#### `null` — ausência intencional de valor

`null` é um valor atribuído deliberadamente para indicar: *"este campo existe, mas não tem valor agora, e isso é intencional"*.

```javascript
const complement = null;
```

**Aplicação.** Um endereço que não foi informado — `complement` tem seu valor igual a `null`. O campo existe, mas o valor não foi disponibilizado. A ausência é intencional.

**Justificativa.** Representar ausência de forma explícita e controlada é necessário para distingui-la de ausência acidental. A presença de `null` indica que alguém atribuiu aquele valor deliberadamente.

---

#### `undefined` — ausência não intencional de valor

`undefined` significa que algo existe como nome, mas nenhum valor foi atribuído a ele. O JavaScript atribui `undefined` automaticamente quando uma variável é declarada sem valor atribuído. Isso se aplica a variáveis declaradas com `let` ou `var`. Com `const`, omitir o valor na declaração gera erro de sintaxe — `const` exige atribuição imediata.

```javascript
let description;

console.log(description); // undefined
```

**Aplicação.** `undefined` raramente é atribuído manualmente. Ele aparece quando uma variável foi declarada mas ainda não preenchida, ou quando se acessa algo que não existe — conceito que virá nas próximas aulas.

**Justificativa.** Distinguir *"vazio"* (`null`) de *"não definido"* (`undefined`) é necessário para que o programa saiba se uma ausência é intencional ou simplesmente resultado de uma variável ainda não preenchida. A distinção parece sutil agora, mas se torna importante quando o programa precisa saber se um valor é intencionalmente nulo ou simplesmente inexistente.

---

#### `bigint` — números inteiros de precisão arbitrária

`bigint` existe para números inteiros maiores do que o `number` consegue representar com precisão. Um `number` tem um limite máximo seguro; acima dele, operações matemáticas começam a produzir resultados incorretos. `bigint` não tem esse limite.

```javascript
const protocol = 9007199254740999n;
```

O sufixo `n` ao final do literal é o que torna o valor um `bigint`.

**Aplicação.** Identificadores de banco de dados gerados por sistemas de grande escala, cálculos financeiros de alta precisão, criptografia. `bigint` não será utilizado nas primeiras aulas; ele aparece neste inventário porque faz parte do sistema de tipos da linguagem.

**Justificativa.** O tipo `number` usa um formato de ponto flutuante de 64 bits que não consegue representar todos os inteiros acima de um certo limite. `bigint` preenche essa lacuna para casos que exigem precisão absoluta em números muito grandes.

---

#### `symbol` — identificador único e irrepetível

Um `symbol` é um valor criado para ser único. Dois símbolos criados com a mesma descrição são, mesmo assim, diferentes um do outro.

```javascript
const primary = Symbol('id');

const foreign = Symbol('id');
```

**Aplicação.** Chaves únicas, identificadores que não devem colidir com outras propriedades. `symbol` não será utilizado nas primeiras aulas; ele aparece aqui pelo mesmo motivo que `bigint`.

**Justificativa.** Em sistemas grandes, existe o risco de partes diferentes usarem o mesmo valor sem saber. `symbol` elimina esse risco porque cada símbolo é garantidamente único.

---

## Bloco 3 — Operador `typeof`

### O problema

Já é possível declarar variáveis e identificar os sete tipos primitivos. Em um programa real, porém, nem sempre é imediato determinar o tipo de uma variável — especialmente quando o valor vem de fora do controle do programa, como algo que o usuário digitou ou que chegou de outra parte do sistema.

O JavaScript oferece uma forma de perguntar ao runtime: *"qual é o tipo desse valor agora?"*. Essa pergunta é feita com o operador `typeof`.

---

### Como `typeof` funciona

`typeof` é um operador unário — ele age sobre um único valor e devolve uma `string` descrevendo o tipo daquele valor.

```javascript
const project = 'Tasks';
const amount = 3;
const completed = false;
const date = null;
let description;

console.log(typeof project); // 'string'
console.log(typeof amount); // 'number'
console.log(typeof completed); // 'boolean'
console.log(typeof date); // 'object'
console.log(typeof description); // 'undefined'
```

**Finalidade.** `typeof` é um operador que inspeciona o tipo de um valor em tempo de execução — ou seja, enquanto o programa está rodando.

**Motivação.** Em aulas futuras será necessário tomar decisões com base no tipo de um valor recebido. Antes de operar sobre algo, é possível verificar o que ele é.

**Decisão.** `typeof` em vez de qualquer outra abordagem porque é nativo da linguagem, não exige importação de nada e funciona sobre qualquer valor, inclusive `undefined`.

**Localização.** `typeof` é escrito imediatamente antes do valor ou do nome da variável a ser inspecionada. O resultado é sempre uma `string` com o nome do tipo.

**Contrafactual.** Sem `typeof`, não haveria como perguntar ao runtime qual é o tipo de um valor de origem desconhecida. O programa operaria às cegas sobre dados externos e produziria erros difíceis de diagnosticar.

---

### A armadilha do `null`

Observe a linha de `date` no exemplo acima. `typeof null` retorna `'object'` — não `'null'`.

Isso é um bug histórico da linguagem, introduzido na implementação original de 1995 e nunca corrigido porque corrigi-lo quebraria código que já existia e dependia desse comportamento. O JavaScript optou por manter a compatibilidade com o passado.

A consequência prática é direta: `typeof` não é confiável para detectar `null`. Quando for necessário verificar especificamente se um valor é `null`, a verificação correta é uma comparação direta:

```javascript
const date = null;
console.log(date === null); // true
```

**Finalidade.** `date === null` é uma comparação que verifica se o valor de `date` é especificamente `null`.

**Motivação.** `typeof null` retorna `'object'`, tornando `typeof` inutilizável para este caso específico.

**Decisão.** `===` é o operador de igualdade estrita em JavaScript — operadores de comparação serão explicados na próxima aula. Por ora, o que importa é que `===` compara valor e tipo sem conversão automática.

**Localização.** Esta comparação aparece em qualquer ponto do programa onde é necessário verificar se um valor é especificamente `null`.

**Contrafactual.** Usar `typeof date === 'null'` faz a comparação sempre retornar `false` — porque `typeof null` retorna `'object'`, nunca `'null'`. O bug silencioso passaria despercebido.

---

## Bloco 4 — Template Strings

### O problema

Com uma variável contendo o nome do usuário e a necessidade de exibir uma mensagem que incorpore esse nome, a única saída disponível até agora seria juntar pedaços de texto usando o operador `+`:

```javascript
const name = 'Alex';

const greeting = 'Hello, ' + name + '!';
```

Isso funciona. Mas imagine que a mensagem seja mais longa, com cinco ou seis valores interpolados. A sequência de `+` e aspas se torna difícil de ler e fácil de errar — esquecer um espaço, fechar aspas no lugar errado, perder um `+`.

O JavaScript oferece uma forma mais limpa de construir textos que incorporam valores: as *template strings*.

---

### Sintaxe e funcionamento

Uma *template string* é delimitada por crases — o caractere `` ` `` — em vez de aspas simples ou duplas. Dentro dela, é possível incorporar qualquer expressão usando a sintaxe `${}`. O runtime avalia a expressão e substitui o bloco pelo resultado.

```javascript
const name = 'Alex';

const greeting = `Hello, ${name}!`;
```

**Finalidade.** *Template strings* são strings delimitadas por crases que permitem incorporar valores de variáveis ou resultados de expressões diretamente no texto, sem concatenação manual.

**Motivação.** Quando existem variáveis com valores e é necessária uma forma legível de combiná-los em texto, a concatenação com `+` se torna difícil de ler e fácil de errar à medida que o número de valores cresce.

**Decisão.** Crase em vez de aspas simples ou duplas porque aspas não suportam interpolação.

**Localização.** *Template strings* são empregadas sempre que for necessário construir um texto que depende de valores calculados ou armazenados em variáveis. O `${}` aceita qualquer expressão JavaScript válida — não apenas nomes de variáveis, mas também operações ou qualquer coisa que produza valor.

**Contrafactual.** Usar aspas simples ou duplas e escrever `${name}` dentro delas não produz interpolação — o JavaScript trata o conteúdo como texto literal e exibe `${name}` na tela.

---

### Strings multilinha

Uma segunda vantagem das *template strings* é o suporte nativo a múltiplas linhas. Com aspas comuns, uma quebra de linha dentro do texto gera erro de sintaxe. Com crases, a quebra de linha é preservada como parte do texto.

```javascript
const name = 'Alex';

const message = `Hello, ${name}!
Welcome to the JavaScript course!`;
```

**Finalidade.** *Template strings* multilinhas representam texto que ocupa mais de uma linha sem nenhum caractere especial adicional.

**Motivação.** Textos longos são comuns em sistemas reais — mensagens, descrições, templates de e-mail. Forçar tudo em uma linha prejudica a leitura do código.

**Decisão.** Crase em vez de aspas porque aspas simples e duplas não suportam quebra de linha literal; nenhuma alternativa dentro da sintaxe de strings comuns resolve o problema sem o uso de `\n`.

**Localização.** Empregada em qualquer ponto do código onde o texto a ser representado abrange mais de uma linha e a legibilidade da quebra literal é preferível ao uso de `\n`.

**Contrafactual.** Usar aspas simples e quebrar a linha dentro do texto faz o Node.js apresentar o erro `SyntaxError: Invalid or unexpected token`. A quebra de linha literal não é permitida dentro de aspas.

---

## Bloco 5 — Coerção de Tipos

### O problema

`string` e `number` são tipos distintos. A pergunta natural é: o que acontece quando o JavaScript encontra uma operação entre dois valores de tipos diferentes? A execução é interrompida com erro?

A resposta é: depende. Em muitos casos, o JavaScript não interrompe — ele tenta resolver a situação por conta própria, convertendo um dos valores para um tipo que faça a operação funcionar. Esse comportamento se chama **coerção implícita**.

Em outros momentos, a conversão é feita intencionalmente, de forma controlada e deliberada. Isso se chama **coerção explícita**.

Entender a diferença entre os dois é crítico porque a coerção implícita produz resultados que parecem incorretos sem o conhecimento do mecanismo subjacente.

---

### Coerção implícita

Observe estes três casos:

```javascript
console.log('5' + 1); // '51'
console.log('5' - 1); // 4
console.log(1 + true); // 2
```

#### Primeiro caso — `'5' + 1`

O operador `+` tem dois comportamentos em JavaScript: concatena strings e soma números. Quando um dos lados é `string`, ele escolhe concatenação. O número `1` é convertido para a string `'1'`, e o resultado é a string `'51'` — não o número `6`.

#### Segundo caso — `'5' - 1`

O operador `-` não tem comportamento de concatenação. Ele só sabe subtrair números. Diante de uma `string`, o JavaScript converte `'5'` para o número `5` e realiza a subtração. O resultado é o número `4`.

O mesmo operador `+` se comporta de forma diferente do `-` porque `+` é sobrecarregado — ele serve para duas coisas. Essa assimetria é a fonte de muita confusão.

#### Terceiro caso — `1 + true`

`true` é convertido para `1` e `false` seria convertido para `0`. A soma produz `2`.

---

O padrão que emerge desses três casos:

O JavaScript nunca interrompe a operação por incompatibilidade de tipos — ele sempre tenta encontrar uma conversão que faça sentido. O problema é que o resultado dessa tentativa nem sempre corresponde ao esperado.

**Justificativa.** O JavaScript foi projetado para ser tolerante a erros em um ambiente de navegador, onde interromper a execução causaria uma página quebrada visível ao usuário. Essa tolerância tem um custo: comportamentos surpreendentes que exigem conhecimento explícito para serem antecipados.

---

### Coerção explícita

Coerção explícita é a conversão escrita intencionalmente, usando funções nativas da linguagem. O controle recai sobre o que está sendo convertido e para qual tipo.

Há três formas nativas do JavaScript para isso: `Number()`, `String()` e `Boolean()`.

---

#### `Number()` — converte para número

```javascript
const value = '30';
const converted = Number(value);

console.log(converted); // 30
console.log(typeof converted); // 'number'
```

**Finalidade.** `Number()` é uma função nativa que recebe um valor e devolve sua representação numérica.

**Motivação.** Valores que chegam de formulários ou de fontes externas frequentemente chegam como texto, mesmo quando representam quantidades. A conversão explícita precede qualquer cálculo.

**Decisão.** `Number()` em vez de deixar a coerção implícita agir, porque coerção implícita depende do operador usado e pode produzir resultados inesperados. A conversão explícita documenta a intenção.

**Localização.** `Number()` é empregada em qualquer ponto do código onde um valor de tipo desconhecido ou textual precisa ser tratado como número antes de uma operação matemática.

**Contrafactual.** `Number('abc')` retorna `NaN` — *Not a Number* (Não é um Número), um valor especial que indica falha na conversão. Qualquer operação matemática com `NaN` produz `NaN`. Isso será útil para detectar entradas inválidas em aulas futuras.

---

#### `String()` — converte para texto

```javascript
const amount = 5;
const converted = String(amount);

console.log(converted); // '5'
console.log(typeof converted); // 'string'
```

**Finalidade.** `String()` é uma função nativa que recebe um valor e devolve sua representação textual.

**Motivação.** Números, booleanos e outros valores precisam ser exibidos como texto em mensagens e interfaces. `String()` é a conversão explícita e segura para isso.

**Decisão.** `String()` em vez de concatenação com `+` ou *template string* porque o uso explícito da função documenta que a intenção é exclusivamente converter o tipo, sem construir texto composto.

**Localização.** `String()` é empregada em qualquer ponto do código onde um valor não textual precisa ser tratado como `string` — especialmente antes de exibi-lo ou combiná-lo com outros textos.

**Contrafactual.** Seria possível usar *template string* para o mesmo resultado — `${amount}` produz `'5'`. As duas formas são válidas; `String()` é mais explícita quando a intenção é exclusivamente converter o tipo.

---

#### `Boolean()` — converte para booleano

```javascript
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean('text')); // true
console.log(Boolean('')); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
```

**Finalidade.** `Boolean()` é uma função nativa que recebe qualquer valor e devolve `true` ou `false` conforme a interpretação da linguagem.

**Motivação.** Decisões em programas são binárias. Em aulas futuras serão escritas condições que precisam avaliar se um valor *"conta como verdadeiro"* ou *"conta como falso"*. `Boolean()` torna essa avaliação explícita e visível.

**Decisão.** `Boolean()` em vez de depender da coerção implícita em estruturas de decisão porque a conversão explícita antecipa e documenta o comportamento, evitando resultados inesperados.

**Localização.** `Boolean()` é empregada em qualquer ponto onde é necessário saber se um valor é considerado verdadeiro ou falso pela linguagem — especialmente antes de estruturas de decisão, que serão introduzidas na próxima aula.

**Contrafactual.** Sem `Boolean()`, a avaliação de verdadeiro ou falso ocorreria implicitamente dentro das estruturas de decisão, sem que o resultado fosse visível ou previsível para quem lê o código.

---

Essa distinção entre valores que se comportam como `true` e valores que se comportam como `false` tem nome na linguagem: valores **truthy** e **falsy**. Os valores *falsy* são um conjunto pequeno e fixo: `0`, `''` (string vazia), `null`, `undefined`, `NaN` e `false`. Todo o resto é *truthy*. Esse conceito será utilizado extensivamente a partir da próxima aula.
