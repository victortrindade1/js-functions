# Funções: como o Javascript funciona

Agora vc vai aprender JS!

Fonte: https://medium.com/reactbrasil/como-o-javascript-funciona-entendendo-as-fun%C3%A7%C3%B5es-e-suas-formas-de-uso-eb387c7fa138

<!-- TOC -->

- [Funções: como o Javascript funciona](#funções-como-o-javascript-funciona)
  - [Funções como objetos de primeira classe](#funções-como-objetos-de-primeira-classe)
  - [Definindo funções](#definindo-funções)
    - [Function declarations](#function-declarations)
    - [Function expressions](#function-expressions)
    - [Arrow functions](#arrow-functions)
    - [Function constructor](#function-constructor)
    - [Callback functions](#callback-functions)
  - [Parâmetros de funções](#parâmetros-de-funções)
    - [Parâmetros X Argumentos](#parâmetros-x-argumentos)
  - [Invocando funções](#invocando-funções)
    - [Invocando funções como funções](#invocando-funções-como-funções)
    - [Invocando como método](#invocando-como-método)
    - [Invocando como construtor](#invocando-como-construtor)
    - [Invocando com os métodos call e apply](#invocando-com-os-métodos-call-e-apply)
    - [Como passar parâmetros e ainda assim não invocar a função](#como-passar-parâmetros-e-ainda-assim-não-invocar-a-função)
  - [arguments e this](#arguments-e-this)
  - [Immediately-invoked function expression(IIEF)](#immediately-invoked-function-expressioniief)

<!-- /TOC -->

## Funções como objetos de primeira classe

Funções na verdade são objetos do tipo _Function_, ou, objetos de primeira
classe. Tudo q um objeto faz, uma função faz. Funções são basicamente tudo em
JS, tirando o código de execução global.

As funções podem ser:

- criadas de forma literal: `function myFunction() {}`
- passadas como parâmetro de outras funções:

  ```javascript
  function foo() {}

  function bar(foo) {}
  ```

- propriedade de objeto: `obj.myFunction = function() {};`
- retornadas como resultado de uma função:
  ```javascript
  function fooBar() {
    return function() {};
  }
  ```
- possuir propriedades atribuidas dinamicamente:

  ```javascript
  function fooBar() {
    fooBar.myProp = 0;
  }

  // Tb poderia ser:
  const fooBar = function() {};

  fooBar.myProp = 0;
  ```

Tudo isso acima prova q função va verdade é objeto!

## Definindo funções

Podemos definir funções como:

- Function declarations
- Function expressions
- Arrow functions
- Function constructor
- function generator (estes não entendi, tb nunca devo usar)

### Function declarations

O jeito mais comum. Vc escreve a palavra function.
Ex: `function fooBar() {}`

Se vc declarar uma função como function declaration, vc pode fazer isto em qqr
posição no código, pois o engine do JS capta toda palavra `function` e já
carrega na memória antes mesmo de rodar o código.

### Function expressions

- Declarando como variável: `const foo = function() {}`
- Passando como argumento: `fooBar(function() {})`

> Em function expressions, o nome da função é opcional!

Diferente de function declaration, aqui a posição no código influencia. O código
abaixo daria erro:

```javascript
fooBar();

let fooBar = function() {};
```

Aqui daria certo:

```javascript
let fooBar = function() {};

foobar();
```

### Arrow functions

Foram introduzidos no ES6. É um jeito mais curto do `function expressions`.

```javascript
const fooBar = (a, b) => {};

// é o mesmo que:
const fooBar = function(a, b) {};
```

Diferenciais do arrow function:

- não precisa da palavra-chave `function`
- não precisa da palavra-chave `return` se for uma linha ou não tiver valor de retorno
- não precisa de estar envolto a {} se for apenas uma linha
- Há um novo operador =>
- Se tiver apenas 1 argumento, os () são opcionais

### Function constructor

É um jeito q ninguém usa, usando `new`.

```javascript
const fooBar = new Function("a", "b", "return a + b");
```

Aqui prova muito q function é objeto!

### Callback functions

Callbacks não são uma forma de definir funções, e nem um tipo novo. É apenas uma
forma de uso de qqr uma das formas acima.

Os callbacks são extremamente usados. São nada mais nada menos que funções
passadas como parâmetros de outras funções, para q em dado momento seja
invocada.

Sempre q vc ver um parâmetro chamado _callback_ numa função, significa que vc
deverá passar uma nova função q será invocada em dado momento no contexto da
função.

Ex com uma requisição AJAX clássica:

```javascript
ajaxRequest("/data", function() {
  //Faça algo quando a requisição estiver completo
});
```

## Parâmetros de funções

Parâmetros são as variáveis declaradas na definição da função.
Existem três tipos de parâmetro:

- Parâmetros simples

```javascript
function fooBar(a, b) {}
```

- Default parameter
  - Vc diz o valor da variável, caso ela não tenha sido passada na chamada da função

```javascript
function fooBar(a = 1, b = "foobar") {}
```

- rest parameter (...)
  - Imagina q vc nem sabe a qnt de argumentos que será passada pra função qnd
    for invocada. O rest parameter coloca tudo nesse parâmetro ...
  - Precisa vir com um nome
  - Precisa ser o último parâmetro.

```javascript
function fooBar(a, ...b) {
  console.log(a);
  console.log(b);
}

fooBar(1, 2, 3, 4, 5, 6); // Retorna: a = 1; b = [2,3,4,5,6]
```

### Parâmetros X Argumentos

Parâmetros e argumentos parecem msm coisa, mas não é.

- Parâmetro: variáveis passadas no momento q _declara_ a função
- Argumento: variáveis passadas no momento q _invoca_ a função

Se vc reparar o exemplo:

```javascript
function fooBar(a, ...b) {
  console.log(a);
  console.log(b);
}

fooBar(1, 2, 3, 4, 5, 6); // Retorna: a = 1; b = [2,3,4,5,6]
```

Vc verá q a função `fooBar` possui mais argumentos q parâmetros. Isto pode. Não
poderia o contrário (ter mais parâmetros q argumentos).

## Invocando funções

Temos 5 maneiras de invocar uma função:

- como funções
- como método
- como um construtor
- com o método Apply
- com o método Call

### Invocando funções como funções

É o jeito simples, invocando pelo nome da função.

```javascript
function fooBar() {} // Define

fooBar(); // Invoca
```

### Invocando como método

Um método é basicamente uma função que foi atribuída para a propriedade de um objeto. Quando invocamos uma função dessa forma dizemos que invocamos uma função como um método.

```javascript
const obj = {};

obj.foobar = function(a, b) {};

console.log(obj.foobar(a, b));
```

### Invocando como construtor

Aqui não estou me referindo à definição `function constructor`, e sim em
`constructor function`. Estou me referindo em como invocar uma função como
construtor. Assim:

```javascript
function fooBar() {}

const foobar = new fooBar();
```

Qnd invocamos uma função como constructor, temos o seguinte comportamento:

- Um novo objeto é criado e atribuido ao parâmetro this. Neste caso, this pode
  inicializar outras propriedades.
- Se um valor primitivo é retornado pela função ele é ignorado, mas se for um objeto, este objeto é retornado.

Não há nada de especial em invocar como constructor. Pode servir para organizar.

### Invocando com os métodos call e apply

O propósito de call e apply é manipular o contexto onde vai atuar a função.
Vc pode criar uma função cheia de `this`, porém se não houver um contexto pra
`this`, todo `this` vai retornar `undefined`. Tá difícil entender? Veja:

```javascript
const teta = {
  pum: "fooo",
  peido: "baaar"
};

function fooBar() {
  console.log(`${this.pum} tem teta lá no ${this.peido}`);
}

fooBar(); // Retorna: "undefined tem teta lá no undefined"

fooBar.call(teta); // Retorna: "fooo tem teta lá no baaar"

fooBar.apply(teta); // Retorna: "fooo tem teta lá no baaar"
```

Basicamente a diferença entre call e apply é q apply recebe os argumentos por
array, e call recebe um por um. Ex: `fooBar.call(1,2,3)`.

### Como passar parâmetros e ainda assim não invocar a função

Se eu precisar chamar função usando parâmetro nela, eu tenho q construir uma arrow function antes dela, para a função não executar no momento q renderiza. Está bem confusa essa explicação, né?! Vamos ver na prática:

Imagina q ao apertar um botão, rode uma função `myFunction`:

```javascript
export default class Foobar extends Component {
  render() {
    return (
      <Container>
        <Button onPress={this.myFunction}>Foo</Button>
      </Container>
    );
  }
}
```

Agora, ao incrementar myFunction, agora ela precisa de um parâmetro (foo): `onPress={this.myFunction(foo)}`

Se colocar desse jeito com o parâmetro, a função vai rodar qnd renderizar, pois só da função ter parênteses, já roda. Para não acontecer isto, deve-se colocar desta maneira: `onPress={() => this.myFunction(foo)}`.

Jeito correto:

```javascript
export default class Foobar extends Component {
  render() {
    return (
      <Container>
        <Button onPress={() => this.myFunction(foo)}>Foo</Button>
      </Container>
    );
  }
}
```

## arguments e this

Qnd vc invoca uma função, vc pode usar parâmetros nativos `arguments` e `this`.
`arguments` retorna um objeto com todos os argumentos invocados:

```javascript
function fooBar(a, b) {
  console.log(arguments);
}

fooBar(2, 3); // Retorna: { "0":2, "1":3 }
```

O arguments sempre retorna um objeto com todo o conteúdo dos argumentos.

Se vc reparar, o rest parameter faz exatamente a msm coisa. Portanto, assim q
veio o ES6, o arguments foi substituido.

O `this` é um facilitador pra acessar as propriedades da instância superior a
função. Por exemplo, uma função método (função dentro de objeto) pode pegar uma
propriedade do objeto pelo this.

```javascript
const foo = {
  name: "gay",
  idade: "tetas",

  fooBar() {
    console.log(`${this.name} tem muitas ${this.idade}`);
  }
};

foo.foobar(); // "gay tem muitas tetas"
```

## Immediately-invoked function expression(IIEF)

Se vc envolve a função em parênteses e coloca no final (), ela será invocada
imediatamente após sua definição.

Ex:

```javascript
(function() {})();
```

Tb daria certo se em vez de estar envolto a parenteses, fosse assim:

```javascript
-(function() {})();
+(function() {})();
!(function() {})();
~(function() {})();
```
