# Parte 2

## Bloco 1 — Variáveis

### O problema

Imagine que você está seguindo uma receita e precisa anotar quantos ovos já colocou na tigela. Você pega um papel e escreve: "*ovos usados: 3*". Depois de colocar mais um, você rasura e escreve "*ovos usados: 4*".

Esse papel com o valor anotado é o que um programa chama de variável: um nome que aponta para um valor guardado na memória do computador, de forma que você possa consultá-lo e, quando necessário, alterá-lo.

Em JavaScript, toda variável precisa ser declarada antes de ser usada. Declarar significa dizer ao runtime: "*reserve um espaço com esse nome*". A linguagem oferece três palavras para fazer essa declaração: `var`, `let` e `const`.

---

### `var` — a declaração original

`var` é a forma mais antiga de declarar variáveis em JavaScript. Ela existe desde a criação da linguagem e você a encontrará em código escrito antes de 2015. O motivo de estudá-la não é para usá-la, mas para que você consiga ler esse código sem estranheza.

O problema central de `var` é o seu escopo. Escopo é a região do programa onde uma variável existe e pode ser acessada. `var` tem escopo específico e isso gerava comportamentos contraintuitivos que causavam problemas difíceis de rastrear.

Em código novo, `var` está proibida. Você não a escreverá em nenhum momento deste curso.

---

### `let` — reatribuição com escopo seguro

`let` foi introduzida em 2015 junto com uma revisão importante da linguagem. Ela resolve o problema de escopo do `var`: uma variável declarada com `let` existe apenas no bloco onde foi criada. Um bloco é qualquer região delimitada por chaves `{ }`.

Use `let` quando o valor da variável precisar ser substituído ao longo do programa.

Exemplo concreto: você está contando quantas tarefas foram concluídas. Esse número começa em zero e aumenta a cada tarefa marcada como feita. O valor muda, então `let` é a declaração adequada.

``` javascript
let completed = 0;

completed = 1;

completed = 2;
```

**Finalidade**: declarar uma variável cujo valor pode ser substituído posteriormente.

**Motivação**: existir neste ponto porque o valor precisa mudar conforme o programa avança.

**Decisão técnica**: `let` em vez de `var` porque o escopo de bloco é previsível e seguro; `let` em vez de `const` porque haverá reatribuição.
Localização e uso: a variável `completed` existe a partir da linha de declaração até o fim do bloco que a contém. Qualquer linha dentro desse bloco pode lê-la ou substituí-la.

**Contrafactual**: se você omitir a declaração e tentar usar o nome diretamente, o Node.js interrompe a execução com `ReferenceError: completed is not defined`. Se usar `var` no lugar, o comportamento é funcionalmente igual neste exemplo simples, mas o escopo se torna imprevisível em estruturas mais complexas, o que será visto nas próximas aulas.

---

### `const` — o padrão para valores que não mudam

`const` também foi introduzida em 2015 e tem o mesmo escopo de bloco que `let`. A diferença é que uma variável declarada com `const` não pode ser reatribuída depois da declaração.

`const` é o padrão do curso. Sempre que você for declarar uma variável, começa com `const`. Só muda para `let` se, ao pensar no problema, você concluir que o valor precisará ser substituído.

Exemplo concreto: o nome do projeto é fixo. Esse é um candidato natural a `const`.


``` javascript
const project = 'Tasks';
```

**Finalidade**: declarar um nome que aponta para um valor fixo, protegendo contra substituição acidental.

**Motivação**: a maioria dos valores em um programa bem escrito não precisa mudar. Usar `const` como padrão força você a pensar conscientemente quando um valor realmente muda — esse pensamento é, em si, uma prática de qualidade.

**Decisão técnica**: `const` em vez de `let` porque não haverá reatribuição. `const` não significa que o valor é imutável em todos os sentidos — esse detalhe será visto mais a frente no curso. Por ora, o que importa é: `const` proíbe substituição do nome por outro valor.

**Localização e uso**: `project` existe a partir da linha de declaração até o fim do bloco. Qualquer tentativa de fazer `project = 'Other Things';` depois resulta em `TypeError: Assignment to `const`ant variable`.

**Contrafactual**: se você usar `let` onde `const` seria suficiente, o código funciona — mas você perde a proteção contra reatribuição acidental e envia um sinal errado para quem lê o código: "*esse valor pode mudar*", quando na verdade não muda.

## Bloco 2 — Tipos Primitivos

### O problema

Volte ao papel da receita. Você anotou "*ovos usados: 3*" e "*ingrediente: farinha*". Esses dois valores são fundamentalmente diferentes: um é um número com o qual você pode fazer contas; o outro é um texto que você lê. Se alguém pedisse para você somar "*farinha*" com 3, a pergunta não faria sentido.

Programas lidam com o mesmo problema. Um valor que representa uma quantidade se comporta de forma diferente de um valor que representa um nome, que se comporta de forma diferente de um valor que representa uma resposta de sim ou não. O JavaScript precisa saber com que tipo de valor está lidando para saber o que pode ser feito com ele.

Esses tipos fundamentais — os blocos mais básicos de dado que a linguagem reconhece — são chamados de tipos primitivos.

---

### Os sete tipos primitivos do JavaScript

O JavaScript tem exatamente sete tipos primitivos. Cada um existe por uma razão distinta.

---

#### `string` — texto

Qualquer sequência de caracteres: uma letra, uma palavra, uma frase, um parágrafo inteiro. Pode ser delimitada por aspas simples, aspas duplas ou crase. A crase tem comportamento especial que veremos no Bloco 4.

``` javascript
const name = 'Alex';

const greeting = 'Hello!';
```

Contexto de aplicação: nomes, títulos, descrições, endereços de e-mail, qualquer coisa que o usuário digita ou que o programa exibe como texto.

Justificativa de existência: sem um tipo dedicado a texto, o programa não teria como representar a diferença entre o número 42 e o texto '42' — e essa diferença importa, como veremos no Bloco 5.

---

#### `number` — número

Qualquer valor numérico: inteiro ou decimal. O JavaScript não separa os dois em tipos distintos — ambos são `number`.

``` javascript
const amount = 5;

const price = 19.90;
```

Contexto de aplicação: contagens, preços, índices, resultados de cálculos, qualquer grandeza mensurável.

Justificativa de existência: números precisam de operações matemáticas — soma, subtração, comparação de magnitude. Texto não. Ter tipos separados permite que a linguagem saiba quais operações fazem sentido.

---

#### `boolean` — verdadeiro ou falso

Exatamente dois valores possíveis: `true` e `false`. Nada mais.

``` javascript
const completed = true;

const logged = false;
```

Contexto de aplicação: estados binários — algo está ativo ou inativo, marcado ou não marcado, permitido ou proibido.

Justificativa de existência: decisões em programas são sempre binárias no nível mais fundamental. Esse usuário está autenticado? Sim ou não. Essa tarefa está concluída? Sim ou não. O tipo `boolean` é o tipo das decisões.

---

#### `null` — ausência intencional de valor

`null` é um valor que você atribui deliberadamente para dizer: "*este campo existe, mas não tem valor agora, e isso é intencional*".

``` javascript
const complement = null;
```

Contexto de aplicação: uma endereço que não foi informado `complement` tem seu valor igual a `null`. O campo existe, a intenção de preenchê-lo pode existir ou não, mas o valor não foi disponibilizado.

Justificativa de existência: representar ausência de forma explícita e controlada. Quando você vê `null`, sabe que alguém colocou aquele valor ali de propósito.

---

#### `undefined` — ausência não intencional de valor

`undefined` significa que algo existe como nome, mas nenhum valor foi atribuído a ele. O JavaScript atribui `undefined` automaticamente quando uma variável é declarada sem valor atribuido.

``` javascript
let description;

console.log(description); // undefined
```

Contexto de aplicação: você raramente atribui `undefined` manualmente. Ele aparece quando você lê uma variável que foi declarada mas ainda não preenchida, ou quando acessa algo que não existe — conceito que virá nas próximas aulas.

Justificativa de existência: distinguir "vazio" (`null`) de "não definido" (`undefined`). A distinção parece sutil agora, mas se torna importante quando o programa precisa saber se um valor é intencionalmente nulo ou simplesmente não existente.

---

#### `bigint` — números inteiros de precisão arbitrária

`bigint` existe para números inteiros maiores do que o `number` consegue representar com precisão. Um `number` tem um limite máximo seguro; acima dele, operações matemáticas começam a produzir resultados incorretos. `bigint` não tem esse limite.

``` javascript
let protocol = 9007199254740999n;
```

O sufixo `n` ao final do literal é o que torna o valor um `bigint`.

Contexto de aplicação: identificadores de banco de dados gerados por sistemas de grande escala, cálculos financeiros de alta precisão, criptografia. Você não usará `bigint` nas primeiras aulas; ele aparece neste inventário porque faz parte do sistema de tipos da linguagem.

Justificativa de existência: o tipo `number` usa um formato de ponto flutuante de 64 bits que não consegue representar todos os inteiros acima de um certo limite. `bigint` preenche essa lacuna para casos que exigem precisão absoluta em números muito grandes.

---

#### `symbol` — identificador único e irrepetível

Um `symbol` é um valor criado para ser único. Dois símbolos criados com a mesma descrição são, mesmo assim, diferentes um do outro.

``` javascript
const primary = Symbol('id');

const foreign = Symbol('id');
```

Contexto de aplicação: chaves únicas, identificadores que não devem colidir com outras propriedades. Você não usará `symbol` nas primeiras aulas; ele aparece aqui pelo mesmo motivo que `bigint`.

Justificativa de existência: em sistemas grandes existe o risco partes diferentes usarem o mesmo valor sem saber. `symbol` elimina esse risco porque cada símbolo é garantidamente único.


## Bloco 3 — Operador `typeof`

### O problema

Você sabe declarar variáveis e conhece os sete tipos primitivos. Mas em um programa real, nem sempre você olha para uma variável e sabe imediatamente qual é o seu tipo — especialmente quando o valor vem de fora do seu controle, como algo que o usuário digitou ou que chegou de outra parte do sistema.

O JavaScript oferece uma forma de perguntar ao runtime: "*qual é o tipo desse valor agora?*". Essa pergunta é feita com o operador `typeof`.

---

### Como `typeof` funciona

`typeof` é um operador unário — ele age sobre um único valor e devolve uma `string` descrevendo o tipo daquele valor.

``` javascript
const project = 'Tasks'
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

**Finalidade**: inspecionar o tipo de um valor em tempo de execução — ou seja, enquanto o programa está rodando.

**Motivação**: existe neste ponto porque você precisará, em aulas futuras, tomar decisões com base no tipo de um valor recebido. Antes de operar sobre algo, é possível verificar o que ele é.

**Decisão técnica**: `typeof` em vez de qualquer outra abordagem porque é nativo da linguagem, não exige importação de nada e funciona sobre qualquer valor, inclusive `undefined`.

**Localização e uso**: você escreve `typeof` imediatamente antes do valor ou do nome da variável que quer inspecionar. O resultado é sempre uma `string` com o nome do tipo.

**Contrafactual**: sem `typeof`, você não teria como perguntar ao runtime qual é o tipo de um valor de origem desconhecida. O programa operaria às cegas sobre dados externos e produziria erros difíceis de diagnosticar.

---

### A armadilha do null

Observe a linha de `date` no exemplo acima. `typeof null` retorna `'object'` — não `'null'`.

Isso é um bug histórico da linguagem, introduzido na implementação original de 1995 e nunca corrigido porque corrigi-lo quebraria código que já existia e dependia desse comportamento. O JavaScript optou por manter a compatibilidade com o passado.

A consequência prática é direta: `typeof` não é confiável para detectar null. Se você precisar verificar especificamente se um valor é `null`, a verificação correta é uma comparação direta:

``` javascript
const date = null;
console.log(date === null); // true
```

**Finalidade**: comparar o valor de `date` com `null` usando igualdade.

**Motivação**: existe porque `typeof null` retorna `'object'`, tornando `typeof` inutilizável para este caso específico.

**Decisão técnica**: `===` para comparação — operadores de comparação será explicada na próxima aula. Por ora, registre que `===` é o operador de comparação no JavaScript.

**Localização e uso**: esta comparação aparece em qualquer ponto do programa onde você precisa saber se um valor é especificamente `null`.

**Contrafactual**: se você usar `typeof date === 'null'`, a comparação sempre retorna `false` — porque `typeof null` retorna `'object'`, nunca `'null'`. O bug silencioso passaria despercebido.

---

## Bloco 4 — Template Strings

### O problema

Você tem uma variável com o nome do usuário e quer exibir uma mensagem que incorpora esse nome. Com o que você sabe até agora, a única saída seria juntar pedaços de texto usando o operador matemático `+`:

``` javascript
const name = 'Alex';

const greeting = 'Hello, ' + name + '!';
```

Isso funciona. Mas imagine que a mensagem seja mais longa, com cinco ou seis valores interpolados. A sequência de `+` e aspas se torna difícil de ler e fácil de errar — esquecer um espaço, fechar aspas no lugar errado, perder um `+`.

O JavaScript oferece uma forma mais limpa de construir textos que incorporam valores: as template strings.

---

### Sintaxe e funcionamento

Uma template string é delimitada por crases — o caractere `` ` `` — em vez de aspas simples ou duplas. Dentro dela, você pode incorporar qualquer expressão usando a sintaxe `${}`. O runtime avalia a expressão e substitui o bloco pelo resultado.

``` javascript
const name = 'Alex';

const greeting = `Hello, ${name}!`;
```

**Finalidade**: construir strings que incorporam valores de variáveis ou resultados de expressões sem concatenação manual.

**Motivação**: existe neste ponto porque você já tem variáveis com valores e precisa de uma forma legível de combiná-los em texto.

**Decisão técnica**: crase em vez de aspas simples ou duplas porque aspas não suportam interpolação.

**Localização e uso**: sempre que você precisar construir um texto que depende de valores calculados ou armazenados em variáveis. O `${}` aceita qualquer expressão JavaScript válida — não apenas nomes de variáveis, mas também operações ou qualquer coisa que produza valor.

**Contrafactual**: se você usar aspas simples ou duplas e tentar escrever `${name}` dentro delas, o JavaScript não interpreta como interpolação — trata como texto literal e exibe `${nome}` na tela.

---

### Strings multilinha

Uma segunda vantagem das template strings é o suporte nativo a múltiplas linhas. Com aspas comuns, uma quebra de linha dentro do texto gera erro de sintaxe. Com crases, a quebra de linha é preservada como parte do texto.

``` javascript
const name = 'Alex';

const message = `Hello, ${name}!
Welcome to the JavaScript course!`;
```

**Finalidade**: representar texto que ocupa mais de uma linha sem nenhum caractere especial adicional.

**Motivação**: existe porque textos longos são comuns em sistemas reais — mensagens, descrições, templates de e-mail. Forçar tudo em uma linha prejudica a leitura do código.

**Contrafactual**: se você usar aspas simples e quebrar a linha dentro do texto, o Node.js apresenará o erro `SyntaxError: Invalid or unexpected token`. A quebra de linha literal não é permitida dentro de aspas.

---

## Bloco 5 — Coerção de Tipos

### O problema

Você agora sabe que `string` e `number` são tipos distintos. A pergunta natural é: o que acontece quando o JavaScript encontra uma operação entre dois valores de tipos diferentes? Ele interrompe com erro?

A resposta é: depende. Em muitos casos, o JavaScript não interrompe — ele tenta resolver a situação por conta própria, convertendo um dos valores para um tipo que faça a operação funcionar. Esse comportamento se chama coerção implícita.

Em outros momentos, é você quem decide fazer a conversão, de forma controlada e deliberada. Isso se chama coerção explícita.

Entender a diferença entre os dois é crítico porque a coerção implícita produz resultados que parecem incorretos se você não souber o que está acontecendo.

---

### Coerção implícita

Observe estes três casos:

``` javascript
console.log('5' + 1); // '51'
console.log('5' - 1); // 4
console.log(1 + true); // 2
```

#### Primeiro caso — `'5' + 1`:

O operador `+` tem dois comportamentos em JavaScript: concatena strings e soma números. Quando um dos lados é `string`, ele escolhe concatenação. O número `1` é convertido para a string `'1'`, e o resultado é a string `'51'` — não o número `6`.

#### Segundo caso — `'5' - 1`:

O operador `-` não tem comportamento de concatenação. Ele só sabe subtrair números. Diante de uma `string`, o JavaScript converte `'5'` para o número `5` e realiza a subtração. O resultado é o número `4`.

O mesmo operador `+` se comporta diferente do `-` porque `+` é sobrecarregado — ele serve para duas coisas. Essa assimetria é a fonte de muita confusão.

#### Terceiro caso — `1 + true`:

`true` é convertido para `1` e `false` seria convertido para `0`. A soma produz `2`.

---

O padrão que emerge desses três casos:

O JavaScript nunca interrompe a operação por incompatibilidade de tipos — ele sempre tenta encontrar uma conversão que faça sentido. O problema é que o resultado dessa tentativa nem sempre é o que você esperava.

**Justificativa de existência da coerção implícita**: o JavaScript foi projetado para ser tolerante a erros em um ambiente de navegador, onde interromper a execução causaria uma página quebrada visível ao usuário. Essa tolerância tem um custo: comportamentos surpreendentes que exigem conhecimento explícito para serem antecipados.

---

### Coerção explícita

Coerção explícita é a conversão que você escreve intencionalmente, usando funções nativas da linguagem. Você controla exatamente o que está convertendo e para qual tipo.

Há três forma nativas do JavaScript para isso: `Number()`, `String()` e `Boolean()`.

---

#### `Number()` — converte para número

``` javascript
const value = '30';
const converted = Number(value);

console.log(converted); // 42
console.log(typeof converted); // 'number'
```

**Finalidade**: transformar um valor de outro tipo em número.

**Motivação**: valores que chegam de formulários ou de fontes externas frequentemente chegam como texto, mesmo quando representam quantidades. Antes de fazer qualquer cálculo, você converte explicitamente.

**Decisão técnica**: `Number()` em vez de deixar a coerção implícita agir, porque coerção implícita depende do operador usado e pode produzir resultados inesperados. Conversão explícita documenta a intenção.

**Contrafactual**: `Number('abc')` retorna `NaN` — *Not a Number*, um valor especial que significa que a conversão falhou. Qualquer operação matemática com `NaN` produz `NaN`. Isso será útil para detectar entradas inválidas em aulas futuras.

---

#### `String()` — converte para texto

``` javascript
const amount = 5;
const converted = String(amount);

console.log(converted); // '5'
console.log(typeof converted); // 'string'
```

`Finalidade`: transformar um valor de outro tipo em texto.

`Motivação`: você precisará exibir números, booleanos e outros valores como texto em mensagens e interfaces. `String()` é a conversão explícita e segura para isso.

`Contrafactual`: você poderia usar template string para o mesmo resultado — `${amount}` produz `'5'`. As duas formas são válidas; `String()` é mais explícita quando a intenção é exclusivamente converter o tipo.

---

#### `Boolean()` — converte para booleano

``` javascript
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean('text')); // true
console.log(Boolean('')); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
```

**Finalidade**: transformar qualquer valor em `true` ou `false`.

**Motivação**: decisões em programas são binárias. Em aulas futuras, você escreverá condições que precisam avaliar se um valor "conta como verdadeiro" ou "conta como falso". `Boolean()` torna essa avaliação explícita e visível.

**Localização e uso**: usado em qualquer ponto onde você precisa saber se um valor é considerado verdadeiro ou falso pela linguagem — especialmente antes de estruturas de decisão, que serão introduzidas na próxima aula.

---

Essa distinção entre valores que se comportam como `true` e valores que se comportam como `false` tem nome na linguagem: valores **truthy** e **falsy**. Os valores falsy são um conjunto pequeno e fixo: `0`, `''` (string vazia), `null`, `undefined`, `NaN` e `false`. Todo o resto é truthy. Você usará esse conceito extensivamente a partir da próxima aula.
