# Parte 2

## Bloco 1 — Laço `for`

### A intuição antes da sintaxe

A contagem da metragem de uma parede para instalação de rodapé é um exemplo cotidiano dessa lógica: começa-se no metro zero, avança-se um metro por vez, e para-se ao chegar ao final da parede — digamos, no metro seis. Em cada ponto executa-se uma ação: anota-se a medida, marca-se o corte, o que for. A lógica tem três partes bem definidas:

1. Onde começo — metro zero.
2. Quando paro — quando chegar ao metro seis.
3. Quanto avanço por vez — um metro.

O `for` traduz exatamente essa estrutura para código.

---

### A sintaxe

```javascript
for (let i = 0; i < 6; i++) {
  console.log(i);
};
```

Essa linha entre parênteses — `let i = 0; i < 6; i++` — tem três partes separadas por ponto e vírgula. Cada uma tem uma função distinta.

---

#### Parte 1 — Inicialização: `let i = 0`

**Finalidade.** Declara e define o valor inicial da variável de controle do laço. Aqui, `i` começa em `0`.

**Motivação.** O laço precisa de um ponto de partida. Sem esse valor inicial, não há como saber onde começa a contagem.

**Decisão.** Usa-se `let` e não `const` porque `i` será reatribuído a cada iteração — `const` não permite reatribuição. O nome `i` é convencional para índice (*index* em inglês), mas qualquer nome válido funciona. Em laços aninhados, costuma-se usar `j` para o laço interno.

**Localização.** A inicialização é executada uma única vez, antes de qualquer iteração. O `i` declarado aqui existe apenas dentro do bloco do `for` — não vaza para o escopo externo, porque `let` respeita escopo de bloco.

**Contrafactual.** Se essa parte for omitida, o JavaScript espera que a variável de controle já exista no escopo externo. Se não existir, ocorre um `ReferenceError`. Se existir, o laço usará o valor que ela já tiver — o que raramente é o comportamento desejado.

---

#### Parte 2 — Condição: `i < 6`

**Finalidade.** Define a regra que determina se o laço continua rodando. Antes de cada iteração, essa expressão é avaliada. Se for `true`, o bloco executa. Se for `false`, o laço encerra.

**Motivação.** Sem essa condição, o laço não teria critério de parada.

**Decisão.** Usa-se `<` e não `<=` porque o objetivo é parar antes de chegar em `6`. Com `i < 6`, o laço roda quando `i` é `0`, `1`, `2`, `3`, `4` e `5` — seis iterações. Com `i <= 6`, rodaria uma iteração a mais, quando `i` fosse `6`. Essa distinção entre `<` e `<=` é fonte de um dos erros mais comuns em programação — o chamado *off-by-one* — e será demonstrada na seção de erros desta aula.

**Localização.** A condição é avaliada antes de cada iteração, incluindo a primeira. Se for falsa desde o início, o bloco nunca executa nenhuma vez.

**Contrafactual.** Se a condição for omitida, o JavaScript a interpreta como `true` permanente. O laço nunca encerra por conta própria — o resultado é um laço infinito que trava o processo. Para interrompê-lo no terminal, utiliza-se `Ctrl+C`.

---

#### Parte 3 — Passo: `i++`

**Finalidade.** Atualiza a variável de controle ao final de cada iteração. Aqui, `i++` incrementa `i` em `1`.

**Motivação.** Sem atualização, `i` nunca muda. A condição `i < 6` permaneceria `true` para sempre, pois `i` ficaria preso em `0`. O resultado seria novamente um laço infinito.

**Decisão.** `i++` é equivalente a `i = i + 1`. Também é possível usar `i += 2` para avançar de dois em dois, `i--` para decrementar, ou qualquer outra expressão de atribuição. O passo não precisa ser `1` — depende do problema.

**Localização.** O passo é executado ao final de cada iteração, depois do bloco, antes da próxima avaliação da condição.

**Contrafactual.** Escrever `i--` em vez de `i++` com a condição `i < 6` e valor inicial `0` faz com que `i` vá para `-1`, depois para `-2`, e assim indefinidamente — porque quanto mais diminui, mais `i < 6` continua `true`. Laço infinito garantido.

---

#### O bloco `{ console.log(i) }`

**Finalidade.** Contém as instruções que serão executadas em cada iteração. Aqui, exibe o valor atual de `i` no terminal.

**Motivação.** É o corpo do trabalho. Sem bloco, o laço apenas contaria sem realizar nada útil.

**Decisão.** As chaves `{}` delimitam o bloco. Qualquer instrução pode estar aqui — atribuições, chamadas de função, condicionais, outros laços.

**Localização.** O bloco é executado depois da verificação da condição e antes do passo.

**Contrafactual.** Sem as chaves, apenas a linha imediatamente seguinte ao `for` seria tratada como corpo do laço — comportamento idêntico ao do `if` sem chaves. O curso adota a convenção de sempre usar chaves, sem exceção.

---

### Aplicação típica

O `for` clássico é a estrutura natural para qualquer situação em que o número de execuções é conhecido de antemão. Para exibir os números de `1` a `100` no terminal, o `for` resolve em três linhas. Para somar todos os números de `1` a `50`, declara-se uma variável acumuladora fora do laço e soma-se `i` a ela em cada iteração. A estrutura é sempre a mesma: um ponto de partida, uma condição de parada, um passo de avanço.

---

## Bloco 2 — Laços `while` e `do...while`

### A intuição antes da sintaxe

O `for` é ideal quando o número de repetições é conhecido antes de começar. Mas nem sempre isso é o caso.

O enchimento de uma banheira ilustra bem a diferença: não se sabe de antemão quantos baldes serão necessários — despeja-se um balde, verifica-se o nível da água, e para-se somente quando a banheira estiver cheia. O critério de parada não é um número fixo de repetições: é uma condição verificada a cada rodada.

É exatamente para esse tipo de situação que existe o `while`.

---

### O `while`

```javascript
while (condition) {
  // block is executed while the condition is true
};
```

**Finalidade.** Executa o bloco repetidamente enquanto a condição entre parênteses for `true`. A cada iteração, a condição é reavaliada antes de o bloco executar novamente.

**Motivação.** Existe para os casos em que o número de iterações não é conhecido antecipadamente. A parada depende de algo que acontece durante a execução — uma entrada, um valor calculado, um estado que muda.

**Decisão.** Diferente do `for`, o `while` não tem inicialização nem passo embutidos na sintaxe. Isso é intencional: ele é deliberadamente minimalista. A variável de controle, quando existir, é declarada fora do laço, e o passo é feito manualmente dentro do bloco. Essa separação deixa claro que o `while` não pressupõe contagem — pressupõe apenas uma condição.

**Localização.** A condição é avaliada antes de cada iteração, incluindo a primeira. Se a condição já for `false` na primeira verificação, o bloco nunca executa — nem uma vez.

**Contrafactual.** Se a atualização da variável de controle for omitida dentro do bloco, a condição nunca muda e o laço nunca termina. Esse é o erro mais comum com `while`.

---

Um exemplo concreto: a tarefa é somar números inteiros a partir de `1` e parar no momento em que a soma ultrapassar `20`. O número de somas necessárias não é conhecido de antemão — depende dos valores acumulados. Com `while`:

```javascript
let sum = 0;
let counter = 1;

while (sum <= 20) {
  sum += counter;
  counter++;
};

console.log(sum);
```

`sum` começa em `0`. A cada iteração, o número atual é somado a `sum`, e `counter` avança. O laço para quando `sum` supera `20`. O número de iterações necessárias não precisa ser conhecido antecipadamente — a condição cuida disso.

---

### O `do...while`

```javascript
do {
  // block executed at least once
} while (condition);
```

**Finalidade.** Executa o bloco primeiro e só depois verifica a condição. Se a condição for `true`, repete. Se for `false`, encerra.

**Motivação.** Existe para os casos em que o bloco precisa executar pelo menos uma vez, independentemente da condição. A verificação vem depois da execução, não antes.

**Decisão.** A diferença em relação ao `while` é precisamente a ordem: execução primeiro, verificação depois. No `while`, se a condição for `false` desde o início, o bloco nunca executa. No `do...while`, ele executa ao menos uma vez, garantidamente.

**Localização.** O bloco executa primeiro. A condição é avaliada ao final de cada iteração. O ponto e vírgula após `while (condition)` é obrigatório — o `do...while` é a única estrutura de controle em JavaScript que termina com `;`.

**Contrafactual.** Usar `while` onde se precisava de `do...while` corre o risco de o bloco não executar nenhuma vez quando a condição inicial já é `false`. Isso pode ser um bug silencioso — o programa não lança erro, simplesmente não faz nada.

---

### Quando usar cada um

O `while` é para *"faça isso enquanto a condição for verdadeira, mas só se ela já for verdadeira desde o início"*. O `do...while` é para *"faça isso pelo menos uma vez, e depois continue enquanto a condição for verdadeira"*.

Um contexto clássico do `do...while` é a validação de entrada: pede-se ao usuário que informe um valor, verifica-se se é válido, e repete-se o pedido se não for. Faz sentido executar o pedido ao menos uma vez antes de checar qualquer coisa — afinal, é necessário ter uma resposta para poder validá-la.

---

## Bloco 3 — Laço `for...of`

### A intuição antes da sintaxe

O `for` clássico itera por contagem — o índice, a condição e o passo são controlados manualmente. Funciona para qualquer coisa que possa ser numerada. Mas há situações em que o índice não é relevante. O objetivo é simplesmente passar por cada item de uma sequência e fazer algo com ele, sem gerenciar nenhum contador.

A analogia de uma fila de atendimento ilustra bem: não é necessário saber que a primeira pessoa é a de número `0`, a segunda é a de número `1`, e assim por diante. Basta atender cada uma delas, em ordem, até a fila acabar. O índice não importa — o valor importa.

É para isso que existe o `for...of`.

---

### A sintaxe

```javascript
for (const item of iterable) {
  // block is executed for each value of the iterable
};
```

**Finalidade.** Percorre os valores de um iterável — um a um, em ordem — e executa o bloco para cada um deles.

**Motivação.** Elimina a necessidade de gerenciar manualmente o índice quando ele não é relevante para o problema. O código fica mais direto: declara-se o que fazer com cada valor, sem a cerimônia da inicialização, da condição e do passo.

**Decisão.** Usa-se `const` na declaração da variável de iteração — aqui chamada `item` — porque ela não é reatribuída dentro do bloco. A cada iteração, uma nova vinculação é criada com o próximo valor da sequência. Caso seja necessário modificar a variável dentro do bloco, usa-se `let`, mas isso é raro e geralmente indica que outra abordagem seria mais adequada.

**Localização.** O `for...of` funciona sobre qualquer valor que seja iterável. O conceito completo de iterável será desenvolvido ao longo do curso. Por ora, o iterável disponível nesta aula é a string.

**Contrafactual.** Usar `for...of` sobre um valor que não é iterável — por exemplo, um número — faz o JavaScript lançar um `TypeError` em tempo de execução, informando que o valor não é iterável.

---

### `for...of` sobre uma string

Uma string é uma sequência de caracteres. O `for...of` percorre cada caractere, um por um:

```javascript
const word = 'coffee';

for (const letter of word) {
  console.log(letter);
};
```

O resultado no terminal seria `c`, `o`, `f`, `f`, `e`, `e` — cada caractere em sua própria linha. Nenhum índice foi necessário. Declarou-se o que se queria — cada letra da palavra — e o laço cuidou do resto.

---

### A distinção crítica

Existe outro laço chamado `for...in`. Os nomes são parecidos, mas o comportamento é completamente diferente, e confundi-los é um erro comum.

O `for...of` itera sobre os valores de um iterável. O `for...in` itera sobre as chaves de um objeto — conceito que será lecionado formalmente na Aula 4. Por ora, o que importa registrar é que os dois laços não são intercambiáveis.

---

## Bloco 4 — `break` e `continue`

### A intuição antes da sintaxe

Até aqui, todos os laços percorrem uma sequência do início ao fim, obedecendo a condição de parada. Há situações, porém, em que é necessário interferir no fluxo do laço antes que ele chegue ao fim naturalmente.

Dois cenários distintos:

**Cenário 1**. A busca pela primeira letra maiúscula em uma palavra: assim que encontrada, não há motivo para continuar — a resposta já foi encontrada. Continuar iterando seria trabalho desperdiçado.

**Cenário 2**. A percorrida pelos dígitos de um número para exibir apenas os ímpares: ao encontrar um par, não se quer encerrar o laço — quer-se apenas pular aquela iteração e continuar com a próxima.

Para o primeiro cenário existe o `break`. Para o segundo, o `continue`.

---

### `break`

```javascript
for (let i = 0; i < 10; i++) {
  if (i === 5) {
    break;
  };
  console.log(i);
};
```

**Finalidade.** Interrompe o laço imediatamente, independentemente de a condição de parada ter sido atingida ou não. O controle passa para a primeira instrução após o bloco do laço.

**Motivação.** Evita iterações desnecessárias quando o objetivo já foi alcançado. Encontrado o que se procurava, não há razão para continuar procurando.

**Decisão.** O `break` encerra o laço mais interno em que está inserido. Se houver laços aninhados — um `for` dentro de outro `for` — o `break` encerra apenas o laço imediatamente ao redor dele, não todos os laços de uma vez.

**Localização.** Sempre dentro de um bloco de laço, geralmente dentro de um `if` que define a condição de saída antecipada. Fora de um laço, o `break` gera um erro de sintaxe.

**Contrafactual.** Sem o `break`, o laço acima exibiria `0, 1, 2, 3, 4` e continuaria até `9`. Com o `break` quando `i === 5`, o laço exibe `0, 1, 2, 3, 4` e para — o `console.log(i)` da iteração em que `i` é `5` não chega a executar porque o `break` ocorre antes.

---

### `continue`

```javascript
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) {
    continue;
  };
  console.log(i);
};
```

**Finalidade.** Interrompe a iteração atual e salta imediatamente para a próxima, sem executar o restante do bloco naquela iteração.

**Motivação.** Permite filtrar iterações sem encerrar o laço. Os casos que não interessam são descartados e o laço continua normalmente para os demais.

**Decisão.** Diferente do `break`, o `continue` não encerra o laço — apenas pula o restante do bloco na iteração corrente. No `for` clássico, após o `continue`, o passo ainda é executado e a condição é reavaliada normalmente. No `while`, a condição é reavaliada imediatamente.

**Localização.** Sempre dentro de um bloco de laço, geralmente dentro de um `if`. O código posicionado após o `continue` no mesmo bloco nunca executa na iteração em que o `continue` é acionado.

**Contrafactual.** Sem o `continue`, o `console.log(i)` executaria para todos os valores — pares e ímpares. Com o `continue` nos pares, o `console.log(i)` é pulado quando `i % 2 === 0`, e o terminal exibe apenas `1`, `3`, `5`, `7`, `9`.
