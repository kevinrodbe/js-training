---
layout: lesson
title: "ES6"
permalink: /es6/
---
<!-- .slide: id="es6" -->
<!-- .slide: id="es6-what-is-ecmascript -->
## What is ES5? ES6? ES WHAT?

FIXME: Mention a few lines what ES5, ES6 is. Whats to come next?

---
<!-- .slide: id="es6-classes -->
## Classes

- Makes ES5 prototypical inheritance model function more like a traditional class-based language
- Supports inheritance using the keyword `extend`
- Supports constructors using the keyword `constructor`

```js
class Hamburger {
  constructor() { }   // constructor
  listToppings() { }  // method
}
```

- Methods of the class can be accessed using a class object.

```js
let burger = new Hamburger();
burger.listToppings();
```

- getter and setter are supported using `get` / `set`

```js
class Hamburger {
  ...
  get toppings() {
    return 'topings are ' + this.toppings;
  }
  set toppings(toppingsList) {
    this.toppings = toppingsList;
  }
}

let burger = new Hamburger();
burger.toppings = ['onion', 'tomatoes'];  // calls the setter
console.log(burger.toppings);             // calls the getter and returns topings are ['onions', 'tomatoes']
```

---
<!-- .slide: id="es6-this -->
## `this`

- When called using dot notation an object's method's `this` will be the object, in other cases `this` won't be the object (unless bound)

```js
class Toppings {
  formatToppings() { /* implementation details */ }
  list() {
    return this.formatToppings(this.toppings);
  }
  fetch(){
    getFromServer(function callback(){
      this.formatToppings(); // not gonna work, because this will be undefined
    });
  }
}

const toppings = new Toppings();
let lostThis = toppings.list;
lostThis() // not gonna work
```
- You can use `call`, `bind` or `supply` to pass `this` to change the context.

```js
let foundThis = toppings.list.bind(toppings);
foundThis() // this will work
```

- But there is a better way in ES6

---
<!-- .slide: id="es6-arrow-functions-1 -->
## Arrow Functions (1/2)

- Arrow functions do not set a local copy of `this`, `arguments`, `super`, or `new.target`. When any of those are used inside an arrow function JavaScript uses them from the outer scope.

```js
class Toppings {
  formatToppings() { /* implementation details */ }
  list() {
    return this.formatToppings(this.toppings);
  }
  fetch(){
    getFromServer(function callback(){
      this.formatToppings(); // not gonna work, because this will be undefined
    });
  }
}

class Toppings {
  formatToppings() { /* implementation details */ }
  list() {
    return this.formatToppings(this.toppings);
  }
  fetch(){
    getFromServer(() => {
      this.formatToppings(); // this will work
    });
  }
}
```

---
<!-- .slide: id="es6-classes-2 -->
## Arrow Functions (2/2)

- ES5

```js
items.forEach(function(x) {
  incrementedItems.push(x+1);
});
```

- ES6 

```
items.forEach((x) => {
  incrementedItems.push(x+1);
});

incrementedItems = items.map((x) => { return x+1; });
incrementedItems = items.map((x) => x+1); // for single return expression can simplify
incrementedItems = items.map(x => x+1);   // when a single param can remove parantheses
```

---

## Template Strings
<!-- .slide: id="es6-template-strings -->
- ES6 introduces a new type of string literal that is marked with back ticks (`). These string literals can include newlines, and there is a string interpolation for inserting variables into strings

```js
let name = 'Sam';
let age = 42;

console.log('hello my name is ' + name + ' I am ' + age + ' years old');
console.log(`hello my name is ${name}, and I am ${age} years old`); // Template String
```

FIXME: Show multilines with back ticks. 

---
<!-- .slide: id="es6-inheritance-1 -->
## Inheritance (1/2)

- The class constructor is called when an object is created using the new operator, it will be called before the object is fully created. A constructor is used to pass in arguments to initialize the newly created object.
- The order of object creation starts from its base class and then moves down to any subclass(es).

```js
// Base Class : ES6
class Bird {
  constructor(weight, height) {
    this.weight = weight;
    this.height = height;
  }
  walk() {
    console.log('walk!');
  }
}
// Subclass
class Penguin extends Bird {
  constructor(weight, height) {
    super(weight, height);
  }
  swim() {
    console.log('swim!');
  }
}
// Penguin object
let penguin = new Penguin(...);
penguin.walk(); //walk!
penguin.swim(); //swim!
```

---
<!-- .slide: id="es6-inheritance-2 -->
## Inheritance (2/2)

- Prototypal inheritance was done before class was introduced to JavaScript.  Below is the previous example in prototypal format

```js
// Bird constructor
function Bird(weight, height) {
  this.weight = weight;
  this.height = height;
}
// Add method to Bird prototype.
Bird.prototype.walk = function() {
  console.log("walk!");
};
// Penguin constructor.
function Penguin(weight, height) {
   Bird.call(this, weight, height);
}
// Prototypal inheritance (Penguin is-a Bird).
Penguin.prototype = Object.create( Bird.prototype );
Penguin.prototype.constructor = Penguin;
// Add method to Penguin prototype.
Penguin.prototype.swim = function() {
  console.log("swim!");
};
// Create a Penguin object.
let penguin = new Penguin(50,10);
// Calls method on Bird, since it's not defined by Penguin.
penguin.walk(); // walk!
// Calls method on Penguin.
penguin.swim(); // swim!
```

---
<!-- .slide: id="es6-const-and-let -->
## `const` and `let`

FIXME: Mention a few words about `const` and `let`

```js
var i;
for (i = 0; i < 10; i += 1) {
  var j = i;
  let k = i;
}
console.log(j); // 9
console.log(k); // undefined

// logs 5,5,5,5,5
for(var x=0; x<5; x++) {
  setTimeout(()=>console.log(x), 0)
}
// logs 1,2,3,4,5
for(let x=0; x<5; x++) {
  setTimeout(()=>console.log(x), 0)
}

const myName = 'pat';
let yourName = 'jo';
yourName = 'sam'; // assigns
myName = 'jan';   // error

const literal = {};
literal.attribute = 'test'; // fine
literal = []; // error;

const person = { name: 'Tammy' };
person.name = 'Pushpa'; // OK, name property changed.
person = null;          // "TypeError: Assignment to constant variable.
```

---

## Spread syntax and Rest parameters (`...`)
<!-- .slide: id="es6-spread-syntax -->
### Spread syntax

FIXME: Mention a few words about `spread`

```js
const add = (a, b) => a + b;
let args = [3, 5];
add(...args); // same as `add(args[0], args[1])`, or `add.apply(null, args)`

let cde = ['c', 'd', 'e'];
let scale = ['a', 'b', ...cde, 'f', 'g'];  // ['a', 'b', 'c', 'd', 'e', 'f', 'g']

let mapABC  = { a: 5, b: 6, c: 3};
let mapABCD = { ...mapABC, d: 7};  // { a: 5, b: 6, c: 3, d: 7 }
```

<!-- .slide: id="es6-rest -->
### Rest parameters

FIXME: Mention a few words about `spread`

```js
function add(...numbers) {
  return numbers[0] + numbers[1];
}
add(3, 2);        // 5

const addEs6 = (...numbers) => numbers.reduce((p, c) => p + c, 0);
addEs6(1, 2, 3);  // 6

function print(a, b, c, ...more) {
  console.log(more[0] + ', ' + arguments[0]);
}
print(1, 2, 3, 4, 5); // 4, 1
```

---
<!-- .slide: id="es6-destructering -->
## Destructuring

FIXME: Mention a few words about `Destructuring`

```js
let foo = ['one', 'two', 'three'];
let [one, two, three] = foo;
console.log(one); // 'one'

let myModule = {
  drawSquare: function drawSquare(length) { /* implementation */ },
  drawCircle: function drawCircle(radius) { /* implementation */ },
  drawText: function drawText(text) { /* implementation */ },
};

let {drawSquare, drawText} = myModule;

drawSquare(5);
drawText('hello');

let jane = { firstName: 'Jane', lastName: 'Doe'};
let john = { firstName: 'John', lastName: 'Doe', middleName: 'Smith' }
function sayName({firstName, lastName, middleName = 'N/A'}) {
  console.log(`Hello ${firstName} ${middleName} ${lastName}`)  
}

sayName(jane) // -> Hello Jane N/A Doe
sayName(john) // -> Helo John Smith Doe
```

---
<!-- .slide: id="es6-modules -->
## ES6 Modules

FIXME: Mention a few words about ES6 modules

```js
//  lib/math.js
export function sum (x, y) { return x + y }
export const pi = 3.141593

//  someApp.js
import * as math from "lib/math"
console.log("2π = " + math.sum(math.pi, math.pi))

//  otherApp.js
import { sum, pi } from "lib/math"
console.log("2π = " + sum(pi, pi))

//  lib/mathplusplus.js
export * from "lib/math"
export const e = 2.71828182846
export default (x) => Math.exp(x)

//  someApp.js
import exp, { pi, e } from "lib/mathplusplus"
console.log("e^{π} = " + exp(pi))
```

## Node.js Modules

FIXME: Show how Node.js modules are different