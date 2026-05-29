# Parte 1

## Bloco 1 — Verificação de Pré-requisitos

Vamos verificar três coisas: Node.js, Git e VSCode. Execute cada comando abaixo no terminal do PowerShell.

---

### Verificação 1 — Node.js

``` shell
node --version
```

O resultado esperado é um número de versão iniciado por `24.x.x` ou superior. Exemplo: `v24.1.0`.

---

### Verificação 2 — Git

``` shell
git --version
```

O resultado esperado é um número de versão iniciado por `2.5x.x` ou superior. Exemplo: `git version 2.53.0`.

---

### Verificação 3 — VSCode

``` shell
code --version
```

O resultado esperado é um número de versão iniciado por `1.1xx.x` ou superior. Exemplo: `1.122.0`.

---

## Bloco 2 — Retomada das Categorias de Linguagem

Antes de apresentar o JavaScript, vale ancorar onde ele se encaixa no que você já conhece.

Na módulo anterior, você trabalhou com duas linguagens. Cada uma pertence a uma categoria distinta, e cada categoria tem uma função específica.
**HTML** é uma linguagem de marcação. Ele descreve estrutura. Diz ao navegador o que existe na página: "aqui contém um título", "aqui contém uma lista", "aqui constém uma imagem". HTML não age. Não decide. Não pergunta. Não repete. Apenas descreve o que há.

CSS é uma linguagem de estilização. Ele descreve aparência. Diz ao navegador como as coisas devem parecer: "este título é azul", "este parágrafo possui 16px", "este elemento se encontra à esquerda". CSS também não age. Não decide. Não repete. Apenas descreve como algo se parece.

Existe uma terceira categoria: linguagem de programação. Uma linguagem de programação não descreve — ela instrui. Ela é capaz de tomar decisões ("se isso for verdade, faça aquilo"), repetir operações ("faça isso dez vezes"), manipular dados ("guarde este valor, some com aquele") e executar ações em resposta a eventos.

JavaScript é a linguagem de programação deste módulo. É a linguagem de programação que você irá aprender.
Não há conteúdo novo aqui — só o encaixe do que vem a seguir no mapa que você já tem.

---

## Bloco 3 — O que é JavaScript

Imagine que você construiu a página de uma loja de bicicletas com HTML e CSS. A página tem uma lista de produtos, cada um com nome, imagem e preço. Ela é bonita, bem estruturada, mas é estática: o conteúdo nunca muda, nada responde a cliques, nenhum dado é guardado.

Agora imagine que um usuário clica em "Adicionar ao carrinho". Para que algo aconteça de fato — para que o produto apareça no carrinho, para que o total seja calculado, para que o botão mude de aparência —, alguém precisa decidir o que fazer com aquele clique. HTML não decide. CSS não decide. Quem decide é JavaScript.

JavaScript é uma linguagem de programação criada originalmente para o navegador. Surgiu nos anos 90 com um propósito específico: ser a linguagem que roda dentro das páginas na Web e as torna interativas. Com o tempo, tornou-se a linguagem padrão da Web — todo navegador do mundo a executa nativamente, sem instalação adicional.

A definição operacional que importa para este módulo é esta: JavaScript é capaz de tomar decisões, repetir operações, manipular dados e executar ações. Nenhuma dessas quatro coisas HTML ou CSS conseguem fazer.

Uma distinção importante: JavaScript, assim como qualquer linguagem de programação, é uma especificação — um conjunto de regras que define o que pode ser escrito e o que cada construção significa. Mas uma especificação, por si só, não faz nada. Para que o código escrito em JavaScript realmente execute e produza efeitos, ele precisa de algo que o leia e o transforme em ação. Esse algo tem um nome específico, e é o próximo conceito.

---

## Bloco 4 — O que é um Runtime

Pense em uma receita escrita em papel. A receita descreve com precisão o que fazer: os ingredientes, as quantidades, a sequência de passos. Mas a receita, sozinha, não produz comida. Alguém precisa lê-la e executá-la. A receita é a especificação. O cozinheiro é quem transforma especificação em resultado.

Com JavaScript funciona da mesma forma.

JavaScript é uma linguagem — isso significa que é um conjunto de regras que define o que pode ser escrito e o que cada construção significa. Você pode escrever um arquivo `.js` com instruções precisas. Mas esse arquivo, sozinho, não faz nada. Ele é texto. Para que as instruções produzam efeitos reais — para que um cálculo seja feito, para que um elemento apareça na tela, para que um arquivo seja criado —, alguém precisa ler esse código e transformá-lo em ação.

Esse "alguém" é chamado de **runtime**.

Runtime — do inglês, literalmente "tempo de execução" — é o programa responsável por ler o código escrito em uma linguagem e executá-lo. É ele que interpreta cada instrução e a transforma em algo concreto que acontece na máquina.

A distinção formal é esta:
- A linguagem define o que pode ser escrito e o que cada construção significa.
- O runtime é o programa que lê esse código e o executa.

Sem runtime, o código JavaScript é apenas texto. É o runtime que dá vida a ele.

JavaScript foi criado com um runtime específico em mente. Esse runtime já existia em todo computador que tinha um navegador instalado — e você o usa desde o módulo anterior.

---

## Bloco 5 — O Navegador como Runtime Original de JavaScript

Você já usou o runtime original de JavaScript desde o módulo anterior — só não sabia que ele tinha esse papel.

O navegador não é apenas um programa que exibe páginas. Internamente, ele carrega um componente específico cuja função é ler código JavaScript e executá-lo. O HTML referenciará o JavaScript, o navegador o localizará, passará o código para esse componente interno, e o componente executará.

Esse é o runtime original de JavaScript. Foi para ele que a linguagem foi criada. É por isso que JavaScript roda em qualquer navegador moderno sem nenhuma instalação adicional — Chrome, Firefox, Edge, Safari: todos carregam esse componente internamente.

O que importa para você agora é compreender uma coisa: quando uma página tem JavaScript, é o navegador que o executa. O código não roda na sua máquina diretamente. Ele roda dentro do navegador, que age como intermediário entre o código e o sistema operacional.

Isso tem uma consequência prática importante: o JavaScript que roda no navegador tem acesso ao que o navegador oferece — a página, os elementos HTML, os eventos do usuário. Mas não tem acesso direto ao sistema de arquivos da sua máquina, por exemplo. O navegador impõe esse limite por segurança.

Essa limitação levanta uma pergunta natural: e se você precisar usar JavaScript para algo que não envolve uma página — para ler um arquivo, criar um serviço fora do navegador, acessar um banco de dados? O navegador não serve para isso. É aí que entra o próximo conceito.

---

## Bloco 6 — Node.js como Runtime de JavaScript fora do Navegador

O navegador resolve bem o problema de executar JavaScript dentro de uma página. Mas ele foi projetado para isso — e apenas para isso. Ele não foi feito para ler arquivos do sistema, para escutar conexões de rede, para se comunicar com um banco de dados. Quando você precisa usar JavaScript para essas tarefas, o navegador não é o ambiente adequado.

Em 2009, um engenheiro chamado Ryan Dahl pegou o motor de JavaScript do Chrome — o componente interno que executa o código — e o retirou de dentro do navegador. Construiu em torno dele um conjunto de capacidades que o navegador deliberadamente não tinha: acesso ao sistema de arquivos, criação de serviços externos, leitura de variáveis de ambiente, comunicação com bancos de dados. O resultado foi um runtime que executa JavaScript em qualquer máquina, sem navegador, diretamente no sistema operacional.

Esse runtime é o **Node.js**.

Com Node.js, JavaScript deixa de ser exclusivamente uma linguagem de páginas web e passa a ser uma linguagem de propósito geral. O mesmo código que você escreve — a mesma sintaxe, as mesmas construções — pode agora rodar no seu terminal, em um serviço, em uma tarefa automatizada.

Existem outros runtimes que fazem algo similar — Deno e Bun são dois exemplos — mas Node.js é o padrão de mercado. É o mais antigo, o mais maduro, e é sobre ele que a indústria construiu suas ferramentas ao longo de mais de quinze anos. É por isso que ele é o runtime deste módulo.

Uma distinção que vale fixar agora:
- No navegador, JavaScript tem acesso à página e aos eventos do usuário, mas não ao sistema de arquivos.
- No Node.js, JavaScript tem acesso ao sistema de arquivos, à rede e ao sistema operacional, mas não há página nem eventos de clique.

Os dois são runtimes de JavaScript. Os dois executam a mesma linguagem. Mas cada um oferece um conjunto diferente de capacidades — porque cada um foi construído para um propósito diferente.

---

## Bloco 7 — Primeiro Arquivo JavaScript Executado pelo Node.js

Chegou o momento de escrever e executar o primeiro código deste módulo.

Vamos fazer isso em três passos: criar o diretório de trabalho, criar o arquivo, e executá-lo com Node.js.

---

### Passo 1 — Criar o diretório de trabalho

No terminal do PowerShell, execute:

``` shell
cd ~
```

**Finalidade**. Navega para o diretório inicial do seu usuário — o ponto de partida padrão para projetos pessoais.

**Motivação**. Garante que você está em um local conhecido antes de criar qualquer coisa, independente de onde o terminal estava aberto.

**Decisão técnica**. `~` é o atalho universal para o diretório inicial do usuário atual. No Windows com PowerShell, equivale a C:\Users\username. Usar `~` em vez do caminho completo torna o comando portável entre usuários e sistemas.

**Contrafactual**. Se omitido, o diretório de trabalho seria criado onde quer que o terminal estivesse no momento — possivelmente em um local inesperado.

---

Agora execute:

``` shell
mkdir course/fundamental
```

**Finalidade**. Cria a estrutura de diretórios `course/fundamental` dentro do inicial.

**Motivação**. Estabelece um local fixo e previsível para todo o trabalho desta Parte. Todos os arquivos `.js` que você criar nas próximas aulas vão aqui.

**Decisão técnica**. O nome `fundamental` reflete a nomenclatura do curso. O diretório `course` agrupa todas as partes em um único lugar, evitando que os diretórios de cada parte fiquem espalhados pelo diretório inicial.

**Contrafactual**. Sem um diretório dedicado, os arquivos deste módulo ficariam misturados com outros arquivos do sistema ou de outros projetos, dificultando localização e organização.

---

Agora entre no diretório criado:

``` shell
cd course/fundamental
```

**Finalidade**. Posiciona o terminal dentro do diretório de trabalho do módulo.

**Motivação**. Todo comando executado a partir daqui opera dentro desse diretório. Isso garante que o arquivo criado no próximo passo fique no lugar correto.

**Contrafactual**. Se você criar o arquivo sem entrar no diretório, ele seria criado no local errado e o Node.js não o encontraria pelo caminho relativo usado na execução.

---

### Passo 2 — Criar o arquivo

``` shell
New-Item -ItemType File -Name playground.js
```

**Finalidade**. Cria um arquivo vazio chamado **playground.js** no diretório atual.

**Motivação**. Este arquivo será o espaço de experimentação de toda a primeira fase. O nome **playground** — do inglês, "área de recreação" — é uma convenção para arquivos usados exclusivamente para testar e experimentar código, sem fazer parte de um projeto real.

**Decisão técnica**. A extensão `.js` é obrigatória. É ela que identifica o arquivo como JavaScript — tanto para o Node.js, que vai executá-lo, quanto para o VSCode, que vai ativá-lo com realce de sintaxe e recursos de edição para a linguagem.

**Contrafactual**. Um arquivo sem extensão ou com extensão incorreta — .txt, por exemplo — seria tratado como texto comum pelo Node.js e causaria erro de execução, ou simplesmente seria ignorado pelos recursos de edição do VSCode.

---

### Passo 3 — Abrir no VSCode e escrever o primeiro código

Execute:

``` shell
code .
```

**Finalidade**. Abre o VSCode com o diretório atual como projeto.

**Motivação**. O ponto `.` referencia o diretório atual. Abrir pelo terminal garante que o VSCode reconhece `fundamental` como a raiz do projeto — o que organiza o explorador de arquivos corretamente.

**Contrafactual**. Abrir o VSCode pelo atalho do sistema e navegar manualmente até o arquivo funcionaria, mas não estabeleceria o diretório como projeto — apenas abriria o arquivo isolado, sem o contexto de pasta.

---

Com o VSCode aberto, clique em `playground.js` no explorador de arquivos e escreva exatamente o seguinte:

``` javascript
console.log('Hello World!');
```

Salve o arquivo com `Ctrl + S`.

**Finalidade**. `console.log` é uma instrução que envia um valor para a saída do terminal. O texto entre aspas é o valor enviado.

**Motivação**. Esta é a instrução mais básica de observação em JavaScript — ela permite ver o resultado de qualquer coisa que o código produza. Será usada em toda a primeira fase para verificar o comportamento do código.

**Decisão técnica**. O texto está entre aspas simples. Em JavaScript, aspas duplas e aspas simples são equivalentes para definir texto. A escolha entre elas é de consistência — o importante é abrir e fechar com o mesmo tipo.

**Contrafactual**. Sem `console.log`, o código poderia executar e produzir resultados internamente, mas você não veria nada. É o `console.log` que torna o resultado visível no terminal.

---

### Passo 4 — Executar com Node.js

De volta ao terminal, execute:

``` shell
node playground.js
```

**Finalidade**. Invoca o Node.js e passa o arquivo `playground.js` como argumento. O Node.js lê o arquivo, interpreta o código e o executa.

**Motivação**. Este é o mecanismo fundamental de execução de JavaScript fora do navegador: você chama o runtime diretamente pelo terminal, informando qual arquivo deve ser executado.

**Decisão técnica**. O arquivo é referenciado pelo nome relativo `playground.js`, não pelo caminho absoluto. Isso funciona porque o terminal está posicionado no mesmo diretório onde o arquivo está. Se você estivesse em outro diretório, precisaria do caminho completo ou relativo correto.

**Contrafactual**. Se o Node.js não estivesse instalado, o comando retornaria um erro de "comando não reconhecido". Se o nome do arquivo estivesse errado, o Node.js retornaria um erro informando que o arquivo não foi encontrado.

---

O terminal deve exibir:

``` shell
Hello World!
```
