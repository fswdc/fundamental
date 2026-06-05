# Parte 1

## Bloco 1 — Operadores Aritméticos e de Atribuição

### Analogia

Pense em uma calculadora. Ela recebe dois números e um símbolo — `+`, `-`, `×`, `÷` — e produz um resultado. Em JavaScript, os operadores aritméticos fazem exatamente isso. A diferença é que em vez de apertar teclas, as expressões são escritas em código.

---

### Operadores aritméticos

JavaScript tem seis operadores aritméticos fundamentais:

```javascript
const addition = 10 + 3;
console.log(addition); // 13
const subtraction = 10 - 3;
console.log(subtraction); // 7
const multiplication = 10 * 3;
console.log(multiplication); // 30
const division = 10 / 3;
console.log(division); // 3.3333...
const remainder = 10 % 3;
console.log(remainder); // 1
const exponentiation = 10 ** 3;
console.log(exponentiation); // 1000
```

O que cada um é, por que existe e o que acontece sem ele:

`+` (**adição**) — soma dois valores numéricos. Existe porque adição é a operação mais fundamental de qualquer sistema que manipule quantidades. Sem ele, seria impossível calcular totais, acumular contadores ou avançar em sequências.

`-` (**subtração**) — retira um valor de outro. Existe para calcular diferenças: quanto falta, quanto sobrou, variações entre estados.

`*` (**multiplicação**) — multiplica dois valores. Existe para expressar repetição de adição de forma compacta: em vez de `3 + 3 + 3 + 3`, escreve-se `3 * 4`.

`/` (**divisão**) — divide um valor pelo outro. Existe para repartição e cálculo de médias. Atenção: divisão por zero em JavaScript não gera erro — produz o valor especial `Infinity`. Essa é uma peculiaridade da linguagem que merece atenção.

`%` (**resto**) — retorna o que sobra de uma divisão inteira. Exemplo concreto: `10 % 3` é `1`, porque `10 = 3 × 3 + 1`. Esse operador tem usos práticos importantes: verificar se um número é par ou criar ciclos que voltam ao zero depois de atingir um limite.

`**` (**exponenciação**) — eleva o primeiro valor à potência do segundo. `10 ** 3` é `10 × 10 × 10`, ou seja, `1000`. Existe para expressar crescimentos exponenciais sem escrever multiplicações repetidas.

---

### Incremento e decremento

Há dois operadores especiais que alteram uma variável em exatamente uma unidade:

```javascript
let counter = 5;

counter++; // counter = counter + 1
console.log(counter); // 6
counter--;  // counter = counter - 1
console.log(counter); // 5
```

`++` (**incremento**) — adiciona 1 à variável. Existe porque incrementar um contador de um em um é tão comum em programação que ganhou notação própria.

`--` (**decremento**) — subtrai 1 da variável. Existe pelo mesmo motivo.

Esses operadores têm duas formas: prefixo (`++contador`) e posfixo (`contador++`). A diferença importa quando o resultado da expressão é usado em outro lugar. Em prefixo, a variável é incrementada antes de ser avaliada. Em posfixo, ela é avaliada antes de ser incrementada. Por ora, a forma posfixo deve ser usada sempre isolada em sua própria linha.

---

### Operadores de atribuição

O operador de atribuição simples `=` foi apresentado na aula anterior. JavaScript também oferece formas abreviadas que combinam uma operação aritmética com atribuição:

```javascript
let points = 100;

points += 50; // points = points + 50
console.log(points); // 150
points -= 30; // points = points - 30
console.log(points); // 120
points *= 2; // points = points * 2
console.log(points); // 240
points /= 4; // points = points / 4
console.log(points); // 60
```

**Por que existem essas formas compostas?** Porque escrever `points = points + 50` repete o nome da variável duas vezes. Em variáveis com nomes curtos isso é tolerável; em nomes mais descritivos como `allAccumulatedPoints`, a repetição prejudica a leitura. Os operadores compostos eliminam a redundância sem perder clareza.

**O que acontece se usar `=` no lugar de `+=`?** A variável perde seu valor anterior e recebe apenas o novo valor. `points = 50` substitui; `points += 50` acumula. São comportamentos completamente diferentes.

---

## Bloco 2 — Operadores de Comparação

### Analogia

Imagine a organização de uma fila de espera em um consultório. Antes de chamar alguém, verifica-se: *"esse paciente chegou antes das 10h?"*, *"esse número de senha é igual ao que está na tela?"*. Dois valores são comparados e o resultado é sim ou não. Em JavaScript, os operadores de comparação fazem exatamente isso — recebem dois valores e devolvem `true` ou `false`.

---

### Operadores

```javascript
console.log(5 > 3); // true
console.log(5 < 3); // false
console.log(5 >= 5); // true
console.log(5 <= 4); // false
console.log(5 == '5'); // true
console.log(5 === '5'); // false
console.log(5 != '5'); // false
console.log(5 !== '5'); // true
```

**`>` e `<`** — verificam relação de grandeza entre dois valores. Existem para qualquer comparação de ordem: maior, menor, antes, depois, acima, abaixo.

**`>=` e `<=`** — incluem a igualdade na comparação. A diferença prática: se um sistema libera acesso a partir dos 18 anos, a condição é `idade >= 18`, não `idade > 18`. Com `>`, alguém com exatamente 18 seria excluído.

**O ponto mais importante desta aula: `==` versus `===`**

Esta é uma das decisões técnicas mais consequentes do JavaScript, e sua compreensão precisa ser precisa.

O operador `==` compara valores, mas antes de comparar, tenta converter os dois lados para o mesmo tipo. Isso se chama **coerção implícita**. O operador `===` compara valor e tipo, sem nenhuma conversão.

```javascript
console.log(5 == '5'); // true
console.log(5 === '5'); // false
console.log(0 == false); // true
console.log(0 === false); // false 
console.log(null == undefined); // true
console.log(null === undefined); // false
```

**Por que `==` existe se causa confusão?** Por razões históricas: JavaScript foi criado em 10 dias, em 1995, com intenção de ser uma linguagem permissiva que não quebrasse o navegador facilmente. A coerção implícita era parte dessa permissividade. O `===` foi adicionado depois como alternativa mais rigorosa.

**Regra prática deste curso, sem exceção: `===` e `!==` são sempre os operadores corretos.** `==` e `!=` não devem ser usados em código novo. A coerção implícita do `==` produz resultados que surpreendem até programadores experientes. O `===` é previsível: dois valores são iguais se e somente se têm o mesmo tipo e o mesmo valor.

**O que acontece ao ignorar essa regra e usar `==`?** O código funciona na maioria dos casos — e falha silenciosamente nos casos menos esperados, porque a coerção equipara valores que não deveriam ser equiparados. O erro se torna difícil de rastrear justamente por não gerar mensagem de erro.

---

## Bloco 3 — Operadores Lógicos e Curto-Circuito

### Analogia

Pense em uma catraca de academia que só abre se duas condições forem verdadeiras ao mesmo tempo: a digital deve ser reconhecida e a mensalidade deve estar em dia. Se qualquer uma das duas falhar, a catraca não abre. Agora imagine uma promoção: o desconto é concedido se o cliente for estudante ou se tiver cupom. Basta uma das duas condições ser verdadeira.

Esse raciocínio de "ambas as condições" e "pelo menos uma condição" é o que os operadores lógicos expressam em código.

---

### Os três operadores

```javascript
console.log(true && true); // true
console.log(true && false); // false
console.log(true || false); // true
console.log(false || false); // false
console.log(!true); // false
console.log(!false); // true
```

`&&` (**"E" lógico**) — o resultado é `true` somente se ambos os lados forem `true`. Existe para expressar requisitos simultâneos: acesso liberado se o usuário está autenticado e tem permissão.

`||` (**"OU" lógico**) — o resultado é `true` se pelo menos um lado for `true`. Existe para expressar alternativas: mostrar mensagem de erro se o campo está vazio ou se o valor é inválido.

`!` (**"NÃO" lógico**) — inverte o valor. Existe para negar condições: executar algo se o usuário não está autenticado.

---

### Curto-circuito

Em JavaScript, `&&` e `||` não devolvem necessariamente `true` ou `false` — eles devolvem um dos operandos, e param de avaliar assim que o resultado está determinado.

**`&&`**: avalia o lado esquerdo primeiro. Se for falsy, retorna o lado esquerdo imediatamente e não avalia o lado direito. Se o lado esquerdo for truthy, retorna o lado direito.

```javascript
console.log('John' && 'Jane'); // 'Jane'
console.log(0 && 'Jane'); // 0
```

**`||`**: avalia o lado esquerdo primeiro. Se for truthy, retorna o lado esquerdo imediatamente e não avalia o lado direito. Se o lado esquerdo for falsy, retorna o lado direito.

```javascript
console.log('John' || 'Jane'); // 'John'
console.log(0 || 'Jane'); // 'Jane'
```

**Por que isso importa na prática?** Porque permite escrever atribuições de valor padrão de forma compacta:

```javascript
const name = username || 'Visitor';
```

Se `username` tiver um valor, ele é usado. Se estiver vazio ou indefinido, `'Visitor'` é o fallback. Esse padrão é frequente em código JavaScript.

**O que acontece ao ignorar o curto-circuito?** Em `&&`, se o lado esquerdo for falsy, o lado direito nunca é executado. Isso tem consequência: se o lado direito contiver uma operação importante, ela simplesmente não acontecerá. Esse é um dos erros silenciosos mais comuns em JavaScript — a lógica parece certa, mas parte do código nunca roda porque o curto-circuito a bloqueou.

---

## Bloco 4 — Truthy e Falsy

### Analogia

Em português, quando alguém pergunta *"tem café?"* e a resposta é *"tem um pouquinho"*, ela é tratada como afirmativa, mesmo não sendo um *"sim"* explícito. Se a resposta é *"não sobrou nada"*, é tratada como negativa. O JavaScript faz algo parecido: qualquer valor pode ser avaliado como verdadeiro ou falso em um contexto condicional, mesmo que não seja literalmente `true` ou `false`.

### Os valores falsy

Em JavaScript, exatamente oito valores são considerados **falsy** — ou seja, quando avaliados em contexto booleano, se comportam como `false`:

```javascript
false
0
-0
0n
""
null
undefined
NaN
```

Tudo o mais é **truthy**.

---

## Bloco 5 — Expressões e Statements

Antes de entrar nas estruturas condicionais, é necessário estabelecer uma distinção que vai aparecer repetidamente no curso.

### Analogia

Em português, há dois tipos de construção linguística com papéis diferentes. Uma expressão é algo que produz um valor: *"o preço do ingresso"* — essa expressão pode ser substituída por um número em qualquer frase. Um ***statement*** é uma instrução completa que executa uma ação: *"compre o ingresso"* — não produz um valor, é um comando.

---

### A distinção em JavaScript

Uma **expressão** é qualquer trecho de código que, quando avaliado, produz um valor. Exemplos:

```javascript
console.log(3 + 4); // 7
console.log('Jane'); // 'Jane'
console.log(18 >= 18); // true
console.log(0 || 'Visitor'); // 'Visitor'
```

Um **statement** é uma instrução que executa uma ação, mas não produz um valor que pode ser usado em outro lugar. Exemplos:

```javascript
const x = 10;

if (x > 5) {};
```

**Por que essa distinção importa?** Porque alguns lugares no código aceitam apenas expressões. Tentar usar um `if` onde uma expressão é esperada causa falha. Ao longo do curso, quando surgir a indicação de que algo *"precisa ser uma expressão"*, o significado já estará estabelecido.

O operador ternário, apresentado no Bloco 8, existe precisamente para expressar uma escolha condicional como expressão, não como statement.

---

## Bloco 6 — Estrutura Condicional `if`

### Analogia

Imagine um porteiro de um edifício com uma lista de instruções:

- Se o visitante estiver na lista de autorizados, libere a entrada.
- Se não estiver na lista mas tiver um crachá de serviço, ligue para o morador.
- Caso contrário, peça que aguarde do lado de fora.

Esse raciocínio — verificar uma condição, executar algo se for verdadeira, verificar outra condição se a primeira for falsa, e ter um caminho padrão — é exatamente o que `if`, `else if` e `else` fazem.

---

### Sintaxe

```javascript
const time = 14;

if (time < 12) {
  console.log('Good morning!');
} else if (time < 18) {
  console.log('Good afternoon!');
} else {
  console.log('Good evening!');
};
```

`if (condition)` — a condição entre parênteses é avaliada como truthy ou falsy. Se for truthy, o bloco entre chaves é executado. Se for falsy, o JavaScript pula esse bloco inteiro.

**Por que a condição fica entre parênteses?** Essa é a sintaxe da linguagem — os parênteses delimitam a expressão que será avaliada. Omiti-los é erro de sintaxe.

**Por que o bloco fica entre chaves?** As chaves definem exatamente quais linhas pertencem àquele caminho condicional. Sem chaves, apenas a linha imediatamente seguinte é considerada parte do `if` — e isso causa erros sutis que são difíceis de encontrar. O curso usa chaves sempre, sem exceção, mesmo quando o bloco tem uma única linha.

`else if (condition)` — verificado somente se todas as condições anteriores foram falsas. É possível encadear quantos `else if` forem necessários. O JavaScript para no primeiro que for truthy e executa seu bloco, ignorando todos os seguintes.

`else` — executado somente se nenhuma das condições anteriores foi verdadeira. É o caminho padrão. Não tem condição — se chegou até aqui, executa. É opcional: se não há comportamento padrão, o `else` pode ser omitido.

**O que acontece ao omitir o `else`?** Se nenhuma condição for verdadeira e não houver `else`, nenhum bloco é executado e o programa continua normalmente após a estrutura. Não é erro — é comportamento válido quando não há nada a fazer no caso padrão.

**O que acontece se duas condições forem verdadeiras?** Apenas a primeira é executada. O JavaScript não avalia as seguintes. Por isso a ordem das condições importa:

```javascript
const note = 95;

if (note >= 60) {
  console.log('Approved!');
} else if (note >= 90) {
  console.log('Approved with distinction!');
};
```

Nesse exemplo, `note >= 60` é verdadeira para `95`, então o JavaScript executa o primeiro bloco e ignora o `else if`. A ordem correta é verificar a condição mais restritiva primeiro:

```javascript
if (note >= 90) {
  console.log('Approved with distinction!');
} else if (note >= 60) {
  console.log('Approved!');
} else {
  console.log('Failed!');
};
```

---

## Bloco 7 — Estrutura Condicional `switch`

### Analogia

Imagine um atendente de lanchonete que recebe pedidos. Para cada item do cardápio, ele tem um procedimento diferente: se for hambúrguer, prepara a chapa; se for suco, vai à fruteira; se for sorvete, abre o freezer. Ele não está verificando uma faixa de valores — está verificando qual item específico foi pedido e executando a ação correspondente. O `switch` é a estrutura adequada para esse tipo de decisão.

---

### Sintaxe

```javascript
const dayOfTheWeek = 'Monday';

switch (dayOfTheWeek) {
  case 'Monday':
    console.log('Start of the week.');
    break;
  case 'Friday':
    console.log('Almost weekend.');
    break;
  case 'Saturday':
  case 'Sunday':
    console.log('Weekend.');
    break;
  default:
    console.log('Middle of the week.');
};
```

`switch (expression)` — a expressão entre parênteses é avaliada uma vez e seu valor é comparado com cada `case`. A comparação usa `===` — sem coerção.

`case value:` — se o valor do `case` for estritamente igual à expressão do `switch`, a execução começa nesse ponto. Se nenhum `case` corresponder, o JavaScript vai para o `default`.

`break` — encerra a execução do `switch` e pula para o código após a chave de fechamento. Sem o `break`, a execução continua para o próximo `case`, independentemente da condição. Esse comportamento se chama *fall-through*.

**Por que o `break` existe em vez de parar automaticamente?** O fall-through é deliberado — ele permite que dois `case` compartilhem o mesmo bloco de código, como `Saturday` e `Sunday` no exemplo acima: os dois entram no mesmo `console.log`. Quando esse comportamento não é desejado, o `break` é obrigatório.

**O que acontece ao esquecer um `break`?** O JavaScript não gera erro. A execução segue para o bloco do `case` seguinte, e o seguinte, até encontrar um `break` ou chegar ao fim. Esse é um dos erros mais silenciosos e frustrantes do JavaScript.

`default` — executado quando nenhum `case` corresponde. É o equivalente ao `else` no `if`. Também é opcional.

---

### Critério de seleção entre `switch` e `if`

`switch` é adequado quando se compara uma única variável contra vários valores específicos e discretos. `if` é adequado quando se avaliam faixas de valores, múltiplas variáveis ou condições compostas. Para um sistema que exibe uma ação diferente para cada dia da semana, `switch` é mais legível.

---

## Bloco 8 — Operador Ternário

### Analogia

Pense na resposta a uma pergunta simples de sim ou não: *"está chovendo? então leva o guarda-chuva, senão deixa em casa."* É uma decisão completa expressa em uma única frase. O operador ternário faz isso em código — uma decisão condicional inteira em uma única expressão.

```javascript
const age = 20;
const situation = age >= 18 ? 'Adult.' : 'Child or Adolescent.';
```

A estrutura é sempre: `condition ? value_if_true : value_if_false`

O `?` separa a condição do resultado verdadeiro. O `:` separa o resultado verdadeiro do resultado falso. Os três elementos são obrigatórios — sem qualquer um deles, é erro de sintaxe.

**Finalidade.** O operador ternário produz um de dois valores com base em uma condição, dentro de uma expressão. Isso é o que o distingue do `if`: o ternário é uma expressão, não um statement. Ele produz um valor que pode ser atribuído a uma variável, passado como argumento, ou usado em qualquer lugar onde uma expressão é válida.

**Motivação.** O `if` não pode ser usado onde uma expressão é necessária. Para definir o valor de uma `const` com base em uma condição, o `if` exige mais linhas e uma variável intermediária com `let`:

```javascript
let situation;

if (age >= 18) {
  situation = 'Adult.';
} else {
  situation = 'Child or Adolescent.';
};
```

O ternário colapsa isso em uma linha, mantendo a variável como `const`.

**Decisão.** O ternário é adequado para decisões simples de dois caminhos onde o resultado é um valor. Ternários aninhados — um ternário dentro de outro — não devem ser usados, pois a leitura se torna difícil rapidamente. Para lógica com mais de dois caminhos, `if/else` é mais legível.

**Localização.** O ternário é utilizado em qualquer ponto do código em que uma decisão condicional precisa produzir um valor diretamente: atribuição de `const`, argumento de função, ou qualquer contexto que exija uma expressão.

**Contrafactual.** Sem o ternário, qualquer atribuição condicional de `const` seria impossível, forçando o uso de `let` com atribuição posterior. Isso enfraquece a intenção do código: `const` declara que o valor não muda depois de atribuído, o que é uma informação valiosa para quem lê.

---

## Bloco 9 — Operador de Coalescência Nula

### Analogia

Imagine um formulário de cadastro onde o campo de apelido é opcional. Se o usuário preencheu, o apelido é utilizado. Se deixou em branco — se o valor é genuinamente ausente — o nome completo é usado como fallback. A palavra-chave aqui é "genuinamente ausente": se o usuário digitou `0` ou uma string vazia como apelido, isso é uma escolha, não ausência.

---

### O problema que `??` resolve

O operador `||` pode servir como fallback:

```javascript
const nickname = userNickname || 'Visitor';
```

O problema: `||` usa avaliação truthy/falsy. Isso significa que `0`, `''` e `false` acionam o fallback, mesmo sendo valores legítimos e intencionais.

```javascript
const amount = 0;

const show = amount || 'Not informed';

console.log(show); // 'Not informed'
```

O operador `??` resolve isso. Ele só aciona o fallback se o lado esquerdo for estritamente `null` ou `undefined` — nada mais.

```javascript
const amount = 0;

const show = amount ?? 'Not informed';

console.log(show); // 0
```

```javascript
const nickname = null;

const show = nickname ?? 'Not nickname';

console.log(show); // 'Not nickname'
```

**Finalidade.** `??` fornece um valor padrão especificamente para ausência de valor (`null` ou `undefined`), sem tratar outros valores falsy como ausência.

**Motivação.** `??` existe porque `||` é impreciso como operador de fallback: ele trata `0`, `false` e `''` como ausência, o que frequentemente não é o comportamento desejado. `??` é mais cirúrgico.

**Decisão.** `??` é adequado quando o fallback deve ser acionado apenas por ausência real de valor. `||` é adequado quando qualquer valor falsy deve acionar o fallback. São ferramentas para situações diferentes.

**Localização.** `??` é utilizado em qualquer ponto do código em que um valor padrão precisa ser fornecido de forma segura — em particular, quando `0`, `false` ou `''` são valores válidos e não devem ser substituídos pelo fallback.

**Contrafactual.** Sem `??`, proteger `0` e `false` de serem substituídos pelo fallback exigiria uma verificação explícita:

```javascript
const show = amount !== null && amount !== undefined ? amount : 'Not informed';
```

O `??` colapsa isso em uma expressão limpa.
