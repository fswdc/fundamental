# Parte 1

Considere o seguinte cenário: em três momentos diferentes de um programa, é necessário calcular se uma tarefa está atrasada. O raciocínio é sempre o mesmo — verificar uma data, comparar com a data atual, retornar verdadeiro ou falso. Sem o recurso apresentado nesta aula, a única saída é escrever esse raciocínio três vezes. Se o critério mudar, é preciso lembrar de alterar três lugares. Se houver erro em um deles, o programa se comporta de forma inconsistente.

Funções resolvem esse problema. Uma função é um bloco de código com nome, escrito uma vez e executável quantas vezes forem necessárias, de qualquer lugar do programa, com dados diferentes a cada chamada.

---

## Bloco 1 — Declaração, Expressão e Seta

### A analogia

Uma receita de bolo escrita num caderno tem um nome (*"Bolo de Cenoura"*), recebe ingredientes (farinha, ovos, cenoura) e produz um resultado (o bolo). A receita pode ser usada hoje, amanhã, para duas pessoas ou para dez — ela em si não muda. Foi escrita uma única vez.

Uma função é exatamente isso: uma receita com nome, que recebe entradas e produz uma saída. O código dentro dela é escrito uma vez, mas executado quantas vezes forem necessárias.

---

### As três formas

Em JavaScript, há três formas de criar uma função. Elas chegam ao mesmo resultado final — um bloco de código nomeado e reutilizável — mas se comportam de maneiras diferentes em detalhes importantes apresentados a seguir.

#### Forma 1 — Declaração de função

```javascript
function sum(a, b) {
  return a + b;
};

console.log(sum(3, 5)); // 8
```

**`function sum(a, b) {`** — Esta linha cria uma função chamada `sum`. A palavra `function` é a palavra-chave que instrui o JavaScript a criar uma função. `sum` é o nome atribuído a ela — é por esse nome que ela é chamada em qualquer parte do código. Os parênteses `(a, b)` declaram que esta função espera receber dois valores quando for chamada; esses valores serão acessíveis dentro da função pelos nomes `a` e `b`. A chave `{` abre o corpo da função — tudo que estiver entre `{` e `}` é o código executado quando a função for chamada.

**`return a + b;`** — Esta linha calcula a soma de `a` e `b` e entrega esse valor de volta para quem chamou a função. A palavra `return` é o mecanismo de devolução: ela encerra a execução da função naquele ponto e passa o resultado para fora. A omissão do `return` faz a função executar normalmente, mas devolver `undefined` — o JavaScript não lança erro, simplesmente não há valor de retorno. Isso causa bugs silenciosos.

**`};`** — Fecha o corpo da função. Tudo fora desse par de chaves não faz parte da função.

**`console.log(sum(3, 5));`** — Esta linha chama a função. `sum(3, 5)` instrui o JavaScript a executar o código dentro de `sum`, entregando `3` no lugar de `a` e `5` no lugar de `b`. O resultado retornado — `8` — é então passado para `console.log`, que o exibe no terminal.

**Por que esta forma existe**. A declaração de função é a forma mais explícita e legível. Ela também possui uma característica única chamada **hoisting**: o JavaScript lê o arquivo inteiro antes de começar a executar, e durante essa leitura eleva a definição da função para o topo do escopo. Isso significa que uma função declarada com `function` pode ser chamada antes mesmo da linha onde foi escrita no código. As outras duas formas não têm esse comportamento.

O que acontece quando o `return` é omitido:

```javascript
function sum(a, b) {
  a + b;
};

console.log(sum(3, 5)); // undefined
```

O cálculo acontece, mas o resultado some. Não há mensagem de erro — o JavaScript simplesmente entende que a função não retorna nada, e devolve `undefined`. Este é um dos erros mais frequentes e mais silenciosos.

---

#### Forma 2 — Expressão de função

```javascript
const subtract = function (a, b) {
  return a - b;
};

console.log(subtract(10, 3)); // 7
```

**`const subtract = function (a, b) {`** — Esta linha faz duas coisas ao mesmo tempo. À direita do `=`, o JavaScript cria uma função — igual à declaração, mas sem nome após a palavra `function`. À esquerda, essa função é armazenada na constante `subtract`. `const` cria um identificador que aponta para um valor. Aqui, esse valor é uma função. Os parênteses `(a, b)` e a chave `{` funcionam exatamente como na declaração.

**`return a - b;`** — Calcula `a - b` e devolve o resultado. Mesma lógica da declaração.

**`};`** — Fecha o corpo da função.

**`console.log(subtract(10, 3));`** — Chama a função pelo nome da constante. O JavaScript localiza a constante `subtract`, verifica que ela contém uma função, e a executa com os argumentos `10` e `3`. Resultado exibido: `7`.

**Por que esta forma existe**. A expressão de função torna explícito que uma função é um valor — como um número ou uma string — que pode ser armazenado em variável, passado como argumento ou retornado de outra função.

A diferença crítica em relação à declaração — sem hoisting:

```javascript
console.log(subtract(10, 3));  // ReferenceError

const subtract = function(a, b) {
  return a - b;
};
```

Este código lança um `ReferenceError`. `const subtract` não é processado antes da execução — ele só existe a partir da linha onde foi declarado. Chamar `subtract` antes dessa linha equivale a tentar usar uma constante que ainda não existe.

---

#### Forma 3 — Função de Seta

```javascript
const multiply = (a, b) => {
  return a * b;
};

console.log(multiply(4, 4)); // 16
```

**`const multiply = (a, b) => {`** — Cria uma função e armazena na constante `multiply`. A diferença visual em relação à expressão é a ausência da palavra `function` e a presença da seta `=>` entre os parênteses e a chave. Esta seta é o marcador que define uma função de seta. Os parâmetros `(a, b)` funcionam da mesma forma.

**`return a * b;`** — Calcula o produto e devolve.

**`};`** e o resto — Idênticos à expressão de função.

**Por que esta forma existe**. A função de seta é uma sintaxe mais curta, criada para tornar o código mais conciso, especialmente quando funções são passadas como argumentos — padrão apresentado adiante nesta aula. Ela também possui uma diferença técnica em relação às outras duas formas que será abordada quando o curso a demandar; por ora, o importante é saber que a diferença existe.

Forma abreviada — quando há apenas um parâmetro:

```javascript
const double = x => x * 2;
```

Se há exatamente um parâmetro, os parênteses são opcionais. Se o corpo da função tem apenas uma expressão cujo valor é o retorno, as chaves e o `return` também são opcionais — a expressão à direita da seta é implicitamente o valor retornado. `double(5)` retorna `10`.

---

## Bloco 2 — Parâmetros, Argumentos e Retorno

### A analogia

Uma balança de cozinha tem duas divisões claras: o espaço onde se coloca o alimento e o visor que mostra o peso. O espaço é fixo — está lá na balança, esperando. O alimento colocado muda a cada pesagem. O visor é o resultado.

Em uma função, o parâmetro é o espaço — o nome reservado na definição, que espera receber um valor. O argumento é o alimento — o valor real entregue na chamada. O retorno é o visor — o resultado que a função devolve para fora.

A distinção entre parâmetro e argumento não é apenas terminológica. Ela representa dois momentos diferentes: a definição (quando a função é escrita) e a chamada (quando ela é executada).

---

### Parâmetros e argumentos

```javascript
function introduce(name, age) {
  return `Name: ${name}, Age: ${age}`;
};

console.log(introduce('Ana', 34)); // Name: Ana, Age: 34
console.log(introduce('Timóteo', 32)); // Name: Timóteo, Age: 32
```

`name` e `age` na linha `function introduce(name, age)` são parâmetros. São nomes que existem apenas dentro do corpo da função. Fora dela, não existem — o JavaScript não os encontra se referenciados externamente.

`'Ana'` e `34` na linha `introduce('Ana', 34)` são argumentos. São os valores reais entregues na chamada. O JavaScript associa o primeiro argumento ao primeiro parâmetro, o segundo ao segundo, em ordem posicional.

A função pode ser chamada quantas vezes forem necessárias com argumentos diferentes. Cada chamada é independente. O código da função é executado do zero a cada chamada, com os valores daquela chamada específica.

---

### Argumentos a mais ou a menos

JavaScript não lança erro em nenhum dos dois casos.

#### A menos:

```javascript
function introduce(name, age) {
  return `Name: ${name}, Age: ${age}`;
};

console.log(introduce('Ana')); // Name: Ana, Age: undefined
```

`age` não recebeu argumento. O JavaScript não reclama — simplesmente atribui `undefined` ao parâmetro sem valor. O retorno será `"Name: Ana, Age: undefined"`. Silencioso e potencialmente problemático.

---

#### A mais:

```javascript
function introduce(name, age) {
  return `Name: ${name}, Age: ${age}`;
};

console.log(introduce('Ana', 34, 'additional')); // Name: Ana, Age: 34
```

O terceiro argumento é ignorado. A função não tem parâmetro para recebê-lo, então ele simplesmente desaparece. Sem erro, sem aviso.

---

Esses comportamentos existem por design na linguagem.

---

### Retorno explícito e implícito

Toda função em JavaScript retorna alguma coisa. A questão é: o quê?

#### Retorno explícito

O `return` é usado com um valor especificado:

```javascript
function double(n) {
  return n * 2;
};

const result = double(5);

console.log(result);  // 10
```

---

#### Retorno implícito

O `return` é omitido, ou usado sem valor:

```javascript
function show(message) {
  console.log(message); // 'Hello!'
};

const result = show('Hello!');
console.log(result);  // undefined
```

`show` executa `console.log` — isso acontece de verdade. Mas ela não devolve nenhum valor. Ao tentar capturar o resultado em `result`, o retorno é `undefined`. Isso não é erro — é o comportamento esperado para funções que existem apenas para produzir efeitos, não para calcular valores.

O problema aparece quando o `return` é esquecido numa função que deveria calcular algo:

```javascript
function triple(n) {
  n * 3;
};

console.log(triple(4));  // undefined
```

O cálculo acontece. O resultado some. Nenhum erro é lançado. Este é exatamente o erro silencioso mais comum com funções.

---

### Encerramento antecipado da execução

```javascript
function checkAge(age) {
  if (age < 18) {
    return 'Underage!';
  };

  return 'Adult!';
};
```

Quando `age < 18`, o primeiro `return` é executado e a função para ali. O segundo `return` nem é alcançado. Esse comportamento pode ser usado intencionalmente para encerrar a execução de uma função antes do fim, sem necessidade de estruturas condicionais complexas.

---

## Bloco 3 — Parâmetros Padrão

### A analogia

Considere um formulário de cadastro onde o campo *"país"* já vem preenchido com *"Brasil"*. Se o campo não for alterado, o valor padrão é usado. Se for alterado, o valor digitado substitui o padrão. O formulário não quebra em nenhum dos dois casos.

Parâmetros padrão funcionam da mesma forma: define-se um valor de reserva que entra em ação quando aquele parâmetro não recebe argumento.

---

### Sintaxe e comportamento

```javascript
function greet(name, greeting = 'Hello') {
  return `${greeting}, ${name}!`;
};

console.log(greet('Ana')); // 'Hello, Ana!'
console.log(greet('Timóteo', 'Good morning')); // 'Good morning, Timóteo!'
```

`greeting = 'Hello'` na definição da função declara um **parâmetro padrão**. O valor `'Hello'` é o reserva — ele é usado quando `greeting` não recebe argumento na chamada.

`greet('Ana')` — apenas `name` recebe argumento. `greeting` não recebeu nada, então o JavaScript usa `'Hello'`. Resultado: `'Hello, Ana!'`.

`greet('Timóteo', 'Good morning')` — `greeting` recebeu `'Good morning'`. O padrão é ignorado. Resultado: `'Good morning, Timóteo!'`.

**Por que esta forma existe**. Antes dos parâmetros padrão, a prática era verificar `undefined` manualmente no início da função:

```javascript
function greet(name, greeting) {
  if (greeting === undefined) {
    greeting = 'Hello'
  };

  return `${greeting}, ${name}!`;
}

console.log(greet('Ana')); // 'Hello, Ana!'
console.log(greet('Timóteo', 'Good morning')); // 'Good morning, Timóteo!'
```

Funciona, mas é verboso. O parâmetro padrão é exatamente essa lógica, escrita de forma compacta diretamente na assinatura da função.

---

## Bloco 4 — Funções como Valores

### A analogia

Uma variável pode guardar um número, uma string ou um booleano. Em JavaScript, uma função é um valor como qualquer outro. Ela pode ser guardada em uma variável, passada como argumento para outra função e retornada como resultado de outra função.

Pense em uma agência de empregos. A agência não executa o trabalho ela mesma — ela recebe um profissional (que tem uma habilidade) e o entrega para um contratante, que decide quando e como usar essa habilidade. A agência não se importa com o que o profissional faz especificamente. Ela apenas o repassa.

Uma função que recebe outra função como argumento funciona da mesma forma: não é necessário saber o que a função recebida faz internamente. Ela é executada no momento certo.

---

### Função como argumento

```javascript
function runTwice(action) {
  action();
  action();
};

function sayHello() {
  console.log('Hello!');
};

runTwice(sayHello);
```

`function runTwice(action)` — declara uma função com um parâmetro chamado `action`. Esse parâmetro não tem tipo declarado — JavaScript não exige isso. O que importa é que, dentro do corpo da função, `action` será tratado como uma função e chamado com `action()`.

`action()` — chama o que estiver armazenado em `action`. Os parênteses `()` são o que dispara a execução. Sem eles, `action` é apenas uma referência ao valor — a função não é executada.

`runTwice(sayHello)` — passa `sayHello` como argumento. Observe: sem parênteses após `sayHello`. Escrever `sayHello()` executaria a função ali mesmo e passaria o resultado dela — que é `undefined` — como argumento. Com `sayHello` sem parênteses, passa-se a função em si, sem executá-la.

O resultado é `'Hello!'` exibido duas vezes.

**Por que esta forma existe**. Há situações em que se sabe o que fazer com um valor, mas não qual operação aplicar — porque isso depende de quem está chamando. Ao receber a operação como argumento, a função se torna genérica e reutilizável.

---

### Retornar função

```javascript
function createGreeting(greeting) {
  function greet(name) {
    return `${greeting}, ${name}!`;
  };
  
  return greet;
};

const goodMorning = createGreeting('Good morning');
const goodNight = createGreeting('Good night');

console.log(goodMorning('Ana')); // 'Good morning, Ana!'
console.log(goodNight('Timóteo')); // 'Good night, Timóteo!'
```

`function createGreeting(greeting)` — declara uma função que recebe uma saudação como argumento.

`function greet(name)` — declara uma segunda função dentro do corpo da primeira. Ela usa `greeting`, que vem do parâmetro externo.

`return greet` — retorna a função `greet` sem executá-la. O resultado de chamar `createGreeting` não é uma string — é uma função pronta para ser usada.

`const goodMorning = createGreeting('Good morning')` — chama `createGreeting` com `'Good morning'`. O retorno é a função `greet` com `greeting` fixado em `'Good morning'`. Essa função é armazenada em `goodMorning`.

`const goodNight = createGreeting('Good night')` — faz o mesmo com `'Good night'`. Agora `goodNight` é uma função diferente, com `greeting` fixado em `'Good night'`.

`goodMorning('Ana')` — chama a função armazenada em `goodMorning` com `'Ana'`. Resultado: `Good morning, Ana!`.

`goodNight('Timóteo')` — resultado: `Good night, Timóteo!`.

**Por que esta forma existe**. Retornar funções permite criar versões especializadas de uma função genérica, cada uma com parte dos dados já fixados. Configura-se uma vez e reutiliza-se com comportamento pré-definido.

---

## Bloco 6 — Recursão

### A analogia

Considere a tarefa de contar quantos degraus tem uma escadaria, podendo olhar apenas para o degrau em que se está. A estratégia é: se não há próximo degrau, a resposta é zero. Se há, desce-se um degrau e repete-se a mesma pergunta. Em algum momento chega-se ao fim e os resultados começam a somar de volta.

Recursão é exatamente esse padrão: uma função que resolve um problema resolvendo uma versão menor do mesmo problema, até chegar a um caso tão simples que a resposta é imediata.

---

### Caso base e caso recursivo

Toda função recursiva tem obrigatoriamente duas partes:

**Caso base**: a condição em que a função para de se chamar e retorna um valor direto. Sem caso base, a função chama a si mesma infinitamente até o JavaScript esgotar a memória disponível para chamadas de função e lançar um erro.

**Caso recursivo**: a parte em que a função se chama com uma versão reduzida do problema, aproximando-se do caso base a cada chamada.

---

### O exemplo do fatorial

O fatorial de um número `n` — escrito matematicamente como `n!` — é o produto de todos os inteiros positivos de `1` até `n`.

- `5!` = `5 × 4 × 3 × 2 × 1` = `120`
- `3!` = `3 × 2 × 1` = `6`
- `1!` = `1`
- `0!` = `1` por definição

Observe o padrão: `5!` é `5 × 4!`. E `4!` é `4 × 3!`. O problema maior contém o mesmo problema em tamanho menor. Esse é o sinal de que recursão é aplicável.

```javascript
function factorial(n) {
  if (n === 0) {
    return 1;
  };

  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120
```

`if (n === 0) { return 1; }` — este é o caso base. Quando `n` chega a zero, a função não se chama mais — retorna `1` diretamente. Sem essa linha, a função chamaria `factorial(-1)`, depois `factorial(-2)`, indefinidamente. O exemplo assume `n >= 0`; entradas negativas estão fora do escopo deste exemplo introdutório.

`return n * factorial(n - 1)` — este é o caso recursivo. A função se chama com `n - 1`, aproximando-se do caso base a cada chamada. O resultado dessa chamada interna é multiplicado por `n`.

O que acontece passo a passo com `factorial(5)`:

```shell
factorial(5) → 5 * factorial(4)
factorial(4) → 4 * factorial(3)
factorial(3) → 3 * factorial(2)
factorial(2) → 2 * factorial(1)
factorial(1) → 1 * factorial(0)
factorial(0) → 1 ← base case

factorial(1) → 1 * 1 = 1
factorial(2) → 2 * 1 = 2
factorial(3) → 3 * 2 = 6
factorial(4) → 4 * 6 = 24
factorial(5) → 5 * 24 = 120
```

Cada chamada fica suspensa, esperando o resultado da chamada seguinte. Quando o caso base é atingido, os resultados começam a retornar em cadeia, de dentro para fora.

---

### O erro obrigatório

```javascript
function factorial(n) {
  return n * factorial(n - 1);
};

factorial(5);
```

Sem o caso base, a função nunca para. O JavaScript mantém cada chamada suspensa em memória enquanto espera a próxima. Essa memória se esgota rapidamente e o JavaScript lança:

```shell
RangeError: Maximum call stack size exceeded
```

**Call stack** é a estrutura interna onde o JavaScript empilha as chamadas de função em andamento. Recursão sem caso base empilha chamadas indefinidamente até estourar esse limite. O termo em inglês para esse esgotamento é *stack overflow*.

---

### Limitação importante

Recursão em JavaScript tem um limite prático: a call stack comporta algumas milhares de chamadas, dependendo do ambiente. Para problemas que exigem muitas chamadas recursivas em sequência, a recursão pode estourar a pilha mesmo com caso base correto. Neste curso, recursão é usada em nível introdutório — os casos práticos abordados nas aulas seguintes não exigirão profundidade recursiva que chegue perto desse limite.
