# JavaScript Style Guide.
## 10 правил написания кода на JavaScript

### 1. Массивы
+ #### Для создания массива используйте литеральную нотацию. eslint: `no-array-constructor`
``` js
// плохо
const items = new Array();

// хорошо
const items = [];
```
+ #### Для добавления элемента в массив используйте [Array#push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания.
``` js
const someStack = [];

// плохо
someStack[someStack.length] = 'abracadabra';

// хорошо
someStack.push('abracadabra');
```
+ #### Для копирования массивов используйте оператор расширения `...` .
``` js
// плохо
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// хорошо
const itemsCopy = [...items];
```

### 2. Строки
+ #### Используйте одинарные кавычки `''` для строк. eslint: `quotes`

``` js
// плохо
const name = "Capt. Janeway";

// плохо - литерал шаблонной строки должен содержать интерполяцию или переводы строк
const name = `Capt. Janeway`;

// хорошо
const name = 'Capt. Janeway';
```
+ #### Строки, у которых в строчке содержится более 100 символов, не пишутся на нескольких строчках с использованием конкатенации.

> Почему? Работать с разбитыми строками неудобно и это затрудняет поиск по коду.

``` js
// плохо
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// плохо
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// хорошо
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```

+ #### При создании строки программным путём используйте шаблонные строки вместо конкатенации. eslint: `prefer-template` `template-curly-spacing`

> Почему? Шаблонные строки дают вам читабельность, лаконичный синтаксис с правильными символами перевода строк и функции интерполяции строки.

``` js
// плохо
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// плохо
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// плохо
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// хорошо
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

### 3. Функции
+ #### Используйте функциональные выражения вместо объявлений функций. eslint: `func-style`

> Почему? У объявлений функций есть подъём. Это означает, что можно использовать функцию до того, как она определена в файле, но это вредит читабельности и поддержке. Если вы обнаружили, что определение функции настолько большое или сложное, что мешает понимать остальную часть файла, то, возможно, пришло время извлечь его в отдельный модуль. Не забудьте явно назвать функциональное выражение, независимо от того, подразумевается ли имя из содержащейся переменной (такое часто бывает в современных браузерах или при использовании компиляторов, таких как Babel). Это помогает точнее определять место ошибки по стеку вызовов. (Обсуждение)

``` js
// плохо
function foo() {
  // ...
}

// плохо
const foo = function () {
  // ...
};

// хорошо
// лексическое имя, отличное от вызываемой(-ых) переменной(-ых)
const foo = function uniqueMoreDescriptiveLexicalFoo() {
  // ...
};
```
+ #### Оборачивайте в скобки немедленно вызываемые функции. eslint: `wrap-iife`

> Почему? Немедленно вызываемая функция представляет собой единый блок. Чтобы чётко показать это — оберните функцию и вызывающие скобки в ещё одни скобки. Обратите внимание, что в мире с модулями вам больше не нужны немедленно вызываемые функции.

``` js
// Немедленно вызываемая функция
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```
+ #### Никогда не объявляйте функции в нефункциональном блоке (`if`, `while`, и т.д.). Вместо этого присвойте функцию переменной. Браузеры позволяют выполнить ваш код, но все они интерпретируют его по-разному. eslint: `no-loop-func`

### 4. Стрелочные функции
+ #### Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию. eslint: `prefer-arrow-callback`, `arrow-spacing`

> Почему? Таким образом создаётся функция, которая выполняется в контексте this, который мы обычно хотим, а также это более короткий синтаксис.

> Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить эту логику внутрь её собственного именованного функционального выражения.

``` js
// плохо
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// хорошо
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```
+ #### Если тело функции состоит из одного оператора, возвращающего [выражение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators#%D0%92%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F) без побочных эффектов, то опустите фигурные скобки и используйте неявное возвращение. В противном случае, сохраните фигурные скобки и используйте оператор return. eslint: `arrow-parens`, `arrow-body-style`

> Почему? Синтаксический сахар. Когда несколько функций соединены вместе, то это лучше читается.

``` js
// плохо
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// хорошо
[1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

// хорошо
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});
```

### 5. Классы и конструкторы
+ #### Всегда используйте `class`. Избегайте прямых манипуляций с `prototype`.

> Почему? Синтаксис `class` является кратким и понятным.

``` js
// плохо
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// хорошо
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```
+ #### Используйте `extends` для наследования.

> Почему? Это встроенный способ наследовать функциональность прототипа, не нарушая `instanceof`.

``` js
// плохо
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this.queue[0];
};

// хорошо
class PeekableQueue extends Queue {
  peek() {
    return this.queue[0];
  }
}
```

### 6. Свойства
+ #### Используйте точечную нотацию для доступа к свойствам. eslint: `dot-notation`

``` js
const luke = {
  jedi: true,
  age: 28,
};

// плохо
const isJedi = luke['jedi'];

// хорошо
const isJedi = luke.jedi;
```

+ #### Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

``` js
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```

+ #### Используйте оператор `**` для возведения в степень. eslint: `no-restricted-properties`.

``` js
// плохо
const binary = Math.pow(2, 10);

// хорошо
const binary = 2 ** 10;
```

### 7. Переменные
+ #### Всегда используйте `const` или `let` для объявления переменных. Невыполнение этого требования приведёт к появлению глобальных переменных. Необходимо избегать загрязнения глобального пространства имён. eslint: `no-undef` `prefer-const`

``` js
// плохо
superPower = new SuperPower();

// хорошо
const superPower = new SuperPower();
```
+ #### Используйте объявление `const` или `let` для каждой переменной или присвоения. eslint: `one-var`

> Почему? Таким образом проще добавить новые переменные. Также вы никогда не будете беспокоиться о перемещении `;` и `,` и об отображении изменений в пунктуации. Вы также можете пройтись по каждому объявлению с помощью отладчика, вместо того, чтобы прыгать через все сразу.
+ В первую очередь группируйте `const`, а затем `let`.

> Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

### 8. Операторы сравнения и равенства
+ #### Используйте `===` и `!==` вместо `==` и `!=`. eslint: `eqeqeq`

+ #### Условные операторы, такие как `if`, вычисляются путём приведения к логическому типу `Boolean` через абстрактный метод `ToBoolean` и всегда следуют следующим правилам:
- **Object** соответствует **true**
- **Undefined** соответствует **false**
- **Null** соответствует **false**
- **Boolean** соответствует **значению булева типа**
- **Number** соответствует **false**, если **+0, -0, or NaN**, в остальных случаях **true**
- **String** соответствует **false**, если строка пустая `''`, в остальных случаях **true**
``` js
if ([0] && []) {
  // true
  // Массив (даже пустой) является объектом, а объекты возвращают true
}
```

+ #### Используйте сокращения для булевских типов, а для строк и чисел применяйте явное сравнение.

``` js
// плохо
if (isValid === true) {
  // ...
}

// хорошо
if (isValid) {
  // ...
}

// плохо
if (name) {
  // ...
}

// хорошо
if (name !== '') {
  // ...
}

// плохо
if (collection.length) {
  // ...
}

// хорошо
if (collection.length > 0) {
  // ...
}
```

### 9. Блоки
+ #### Используйте фигурные скобки, когда блок кода занимает несколько строк. eslint: `nonblock-statement-body-position`

``` js
// плохо
if (test)
  return false;

// хорошо
if (test) return false;

// хорошо
if (test) {
  return false;
}
```

+ #### Если блоки кода в условии `if` и `else` занимают несколько строк, расположите оператор `else` на той же строчке, где находится закрывающая фигурная скобка блока `if`. eslint: `brace-style`

``` js
// плохо
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// хорошо
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

### 10. Управляющие операторы
+ #### Если ваш управляющий оператор (`if`, `while` и т.д.) слишком длинный или превышает максимальную длину строки, то каждое (сгруппированное) условие можно поместить на новую строку. Логический оператор должен располагаться в начале строки.

> Почему? Наличие операторов в начале строки приводит к выравниванию операторов и напоминает цепочку методов. Это также улучшает читаемость, упрощая визуальное отслеживание сложной логики.

``` js
// плохо
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}

// плохо
if (foo === 123 &&
  bar === 'abc') {
  thing1();
}

// плохо
if (foo === 123
  && bar === 'abc') {
  thing1();
}

// плохо
if (
  foo === 123 &&
  bar === 'abc'
) {
  thing1();
}

// хорошо
if (
  foo === 123
  && bar === 'abc'
) {
  thing1();
}
```