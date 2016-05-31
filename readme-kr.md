# 함수형 프로그래밍 용어집

> The whole idea of this repos is to try and define jargon from combinatorics and category theory jargon that are used in functional programming in a easier fashion.

> 이 저장소의 목적은 함수형 프로그래밍에서 사용되는 조합론과 범주론의 전문 용어를 쉬운 방식으로 설명하는 것입니다.

*Let's try and define these with examples, this is a WIP—please feel free to send PR ;)*

*함께 이 예제를 설명해봅시다. 언제든지 편하게 PR을 보내주세요 ;)*


## Arity

> The number of arguments a function takes. From words like unary, binary, ternary, etc. This word has the distinction of being composed of two suffixes, "-ary" and "-ity." Addition, for example, takes two arguments, and so it is defined as a binary function or a function with an arity of two. Such a function may sometimes be called "dyadic" by people who prefer Greek roots to Latin. Likewise, a function that takes a variable number of arguments is called "variadic," whereas a binary function must be given two and only two arguments, currying and partial application notwithstanding (see below).

> 번역해야함

```js
const sum = (a, b) => a + b;

const arity = sum.length;
console.log(arity);
// => 2
// The arity of sum is 2
```
---

## Higher-Order Functions (HOF)

> A function which takes a function as an argument and/or returns a function.

> 인자로 함수를 받거나 함수를 반환하는 함수를 의미합니다.

```js
const filter = (pred, xs) => {
  const result = [];
  for (var idx = 0; idx < xs.length; idx += 1) {
    if (pred(xs[idx])) {
      result.push(xs[idx]);
    }
  }
  return result;
};
```

```js
const is = type => x => Object(x) instanceof type;
```

```js
filter(is(Number), [0, '1', 2, null]); //=> [0, 2]
```

## Partial Application

> The process of getting a function with lesser arity compared to the original
function by fixing the number of arguments is known as partial application.

> 인자의 수가 고정 되어있는 원래의 함수에서 원래 함수보다 
낮은 인자 수의 함수를 얻는 과정을 Partial Application라고 합니다.

```js
let sum = (a, b) => a + b;

// partially applying `a` to `40`
let partial = sum.bind(null, 40);

// Invoking it with `b`
partial(2); //=> 42
```

---

## Currying

> The process of converting a function with multiple arity into the same function with an arity of one. Not to be confused with partial application, which can produce a function with an arity greater than one.

> 다수의 인자를 가지는 함수를 하나의 인자를 가진 함수로 변환하는 방법입니다. 많은 인자를 가진 함수에서 함수를 생산할 수 있는 Partial Application과는 다릅니다.

```js
let sum = (a, b) => a + b;

let curriedSum = (a) => (b) => a + b;

curriedSum(40)(2) // 42.
```
---

## Composition

> A function which combines two values of a given type (usually also some kind of functions) into a third value of the same type.

> 번역 해야함

The most straightforward type of composition is called "normal function composition".
It allows you to combines functions that accept and return a single value.

```js
const compose = (f, g) => a => f(g(a)) // Definition
const floorAndToString = compose((val)=> val.toString(), Math.floor) //Usage
floorAndToString(121.212121) // "121"

```

---

## Purity

> A function is said to be pure if the return value is only determined by its
input values, without any side effects.

> 함수가 리턴 값이 입력 값에 의해 결정되는 경우 Side effect없이 리턴하는 함수를 Purity라고 합니다.

```js
let greet = "yo";

greet.toUpperCase(); // YO;

greet // yo;
```

As opposed to:

반대의 경우:

```js
let numbers = [1, 2, 3];

numbers.splice(0); // [1, 2, 3]

numbers // []
```

---

## Side effects

> A function or expression is said to have a side effect if apart from returning a value, it modifies some state or has an observable interaction with external functions.

> 번역 해야 함

```js
console.log("IO is a side effect!");
```
---

## Idempotency

> A function is said to be idempotent if it has no side-effects on multiple
executions with the the same input parameters.

> 같은 매개 변수와 여러 번의 실행에도 아무런 Side effect가 없는 경우에 Idempotency(멱등성)라고 합니다.

`f(f(x)) = f(x)`

`Math.abs(Math.abs(10))`

---

## Point-Free Style

> Writing functions where the definition does not explicitly define arguments. This style usually requires [currying](#currying) or other [Higher-Order functions](#higher-order-functions-hof). A.K.A Tacit programming.

> 정의가 명시적으로 인자를 정의하지 않는 함수를 작성합니다. 이 스타일은 일반적으로 [Currying](#currying) 또는 [Higher-Order functions](#higher-order-functions-hof)가 필요합니다. Tacit programming라고도 불립니다. 번역 다시

```js
// Given
let map = fn => list => list.map(fn);
let add = (a, b) => a + b;

// Then

// Not points-free - `numbers` is an explicit parameter
let incrementAll = (numbers) => map(add(1))(numbers);

// Points-free - The list is an implicit parameter
let incrementAll2 = map(add(1));
```

`incrementAll` identifies and uses the parameter `numbers`, so it is not points-free.  `incrementAll2` is written just by combining functions and values, making no mention of its arguments.  It __is__ points-free.

Points-free function definitions look just like normal assignments without `function` or `=>`.

---

## Contracts

---

## Guarded Functions

---

## Categories

> Objects with associated functions that adhere certain rules. E.g. [monoid](#monoid)

> 특정한 규칙을 따르는 함수와 관련된 오브젝트 입니다. 예를들어 [Monoid](#monoid)가 있습니다.

---

## Value 

> Any complex or primitive value that is used in the computation, including functions. Values in functional programming are assumed to be immutable.

> 함수를 포함하여 계산에 사용되는 복잡하거나 기본적인 값입니다. 함수형 프로그래밍에서 Value는 불변합니다.

```js
5
Object.freeze({name: 'John', age: 30}) // The `freeze` function enforces immutability.
(a) => a
```
Note that value-containing structures such as [Functor](#functor), [Monad](#monad) etc. are themselves values. This means, among other things, that they can be nested within each other.

---

## Constant 

> An immutable reference to a value. Not to be confused with `Variable` - a reference to a value which can at any point be updated to point to a different value.
```js
const five = 5
const john = {name: 'John', age: 30}
```
Constants are referentially transparent. That is, they can be replaced with the values that they represent without affecting the result.
In other words with the above two constants the expression:

```js
john.age + five === ({name: 'John', age: 30}).age + (5)

```
Should always return `true`.

---

## Functor

> An object with a `map` function that adhere to certains rules. `Map` runs a function on values in an object and returns a new object.

Simplest functor in javascript is an `Array`

```js
[2,3,4].map( n => n * 2 ); // [4,6,8]
```

Let `func` be an object implementing a `map` function, and `f`, `g` be arbitrary functions, then `func` is said to be a functor if the map function adheres to the following rules:

```js
func.map(x => x) == func
```

and

```js
func.map(x => f(g(x))) == func.map(g).map(f)
```

We can now see that `Array` is a functor because it adheres to the functor rules!
```js
[1, 2, 3].map(x => x); // = [1, 2, 3]
```

and
```js
let f = x => x + 1;
let g = x => x * 2;

[1, 2, 3].map(x => f(g(x))); // = [3, 5, 7]
[1, 2, 3].map(g).map(f);     // = [3, 5, 7]
```
---

## Pointed Functor
> A functor with an `of` method. `Of` puts _any_ single value into a functor.

Array Implementation: 
```js
  Array.prototype.of = (v) => [v];
  
  [].of(1) // [1]
```

---

## Lift

> Lift is like `map` except it can be applied to multiple functors.

Map is the same as a lift over a one-argument function:

```js
lift(n => n * 2)([2,3,4]); // [4,6,8]
```
Unlike map lift can be used to combine values from multiple arrays:
```
lift((a, b)  => a * b)([1, 2], [3]); // [3, 6]
```

---

## Referential Transparency

> An expression that can be replaced with its value without changing the
behavior of the program is said to be referentially transparent.

Say we have function greet:

```js
let greet = () => "Hello World!";
```

Any invocation of `greet()` can be replaced with `Hello World!` hence greet is
referentially transparent.

---

##  Equational Reasoning

> When an application is composed of expressions and devoid of side effects, truths about the system can be derived from the parts.

---

## Lazy evaluation

> Lazy evaluation is a call-by-need evaluation mechanism that delays the evaluation of an expression until its value is needed. In functional languages, this allows for structures like infinite lists, which would not normally be available in an imperative language where the sequencing of commands is significant.

```js
let rand = function*() {
    while(1<2) {
        yield Math.random();
    }
}
```
```js
let randIter = rand();
randIter.next(); // Each exectuion gives a random value, expression is evaluated on need.
```
---

## Monoid

> A monoid is some data type and a two parameter function that "combines" two values of the type, where an identity value that does not affect the result of the function also exists.

One very simple monoid is numbers and addition:

```js
1 + 1; // 2
```

The data type is number and the function is `+`, the addition of two numbers.

```js
1 + 0; // 1
```

The identity value is `0` - adding `0` to any number will not change it.

For something to be a monoid, it's also required that the grouping of operations will not affect the result:

```js
1 + (2 + 3) == (1 + 2) + 3; // true
```

Array concatenation can also be said to be a monoid:

```js
[1, 2].concat([3, 4]); // [1, 2, 3, 4]
```

The identity value is empty array `[]`

```js
[1, 2].concat([]); // [1, 2]
```
Functions also form a monoid with the normal functional compositon as an operation and the function which returns its input `(a) => a` 


---

## Monad

> A monad is an object with [`of`](#pointed-functor) and `chain` functions. `Chain` is like [map](#functor) except it unnests the resulting nested object.

```js
['cat,dog','fish,bird'].chain(a => a.split(',')) // ['cat','dog','fish','bird']

//Contrast to map
['cat,dog','fish,bird'].map(a => a.split(',')) // [['cat','dog'], ['fish','bird']]
```

You may also see `of` and `chain` referred to as `return` and `bind` (not be confused with the JS keyword/function...) in languages which provide Monad-like constructs as part of their standard library (e.g. Haskell, F#), on [Wikipedia](https://en.wikipedia.org/wiki/Monad_%28functional_programming%29) and in other literature. It's also important to note that `return` and `bind` are not part of the [Fantasy Land spec](https://github.com/fantasyland/fantasy-land) and are mentioned here only for the sake of people interested in learning more about Monads.

---

## Comonad

> An object that has `extract` and `extend` functions.

> `extract`와 `extend` 함수를 가진 오브젝트를 의미합니다.

```js
let CoIdentity = v => ({
    val: v,
    extract: this.v,
    extend: f => CoIdentity(f(this))
})
```

Extract takes a value out of a functor.
```js
CoIdentity(1).extract() // 1
```

Extend runs a function on the comonad. The function should return the same type as the Comonad.
```js
CoIdentity(1).extend(co => co.extract() + 1) // CoIdentity(2)
```
---

## Applicative Functor

> An applicative functor is an object with an `ap` function. `Ap` applies a function in the object to a value in another object of the same type.

```js
[(a)=> a + 1].ap([1]) // [2]
```

---

## Morphism

> A transformation function.

> 무언가로 인해 변화한 함수를 의미합니다.

---

## Isomorphism

> A pair of transformations between 2 types of objects that is structural in nature and no data is lost. 

For example, 2D coordinates could be stored as an array `[2,3]` or object `{x: 2, y: 3}`.
```js
// Providing functions to convert in both directions makes them isomorphic.
const pairToCoords = (pair) => ({x: pair[0], y: pair[1]})

const coordsToPair = (coords) => [coords.x, coords.y]

coordsToPair(pairToCoords([1, 2])) // [1, 2]

pairToCoords(coordsToPair({x: 1, y: 2})) // {x: 1, y: 2}
```

---

## Setoid

> An object that has an `equals` function which can be used to compare other objects of the same type.

Make array a setoid.
```js
Array.prototype.equals = arr => {
    var len = this.length
    if (len != arr.length) {
        return false
    }
    for (var i = 0; i < len; i++) {
        if (this[i] !== arr[i]) {
            return false
        }
    }
    return true
}

[1, 2].equals([1, 2]) // true
[1, 2].equals([0]) // false
```

---

## Semigroup

An object that has a `concat` function that combines it with another object of the same type.

같은 타입인 다른 오브젝트와 결합할 수 있는 `concat` 함수를 가진 오브젝트를 의미합니다.

```js
[1].concat([2]) // [1, 2]
```

---

## Foldable

> An object that has a reduce function that can transform that object into some other type.

> 다른 유형에 해당 오브젝트를 변환 할 수 있는 `reduce` 함수가 있는 오브젝트입니다.

```js
let sum = list => list.reduce((acc, val) => acc + val, 0);
sum([1, 2, 3]) // 6
```

---

## Traversable

---
## Type Signatures

> Often functions will include comments that indicate the types of their arguments and return types.

There's quite a bit variance across the community but they often follow the following patterns:
```js
// functionName :: firstArgType -> secondArgType -> returnType

// add :: Number -> Number -> Number
let add = x => y => x + y

// increment :: Number -> Number
let increment = x => x + 1
```

If a function accepts another function as an argument it is wrapped in parenthesis.

```js
// call :: (a -> b) -> a -> b
let call = f => x => f(x)
```
The letters `a`, `b`, `c`, `d` are used to signify that the argument can be of any type. For this map it takes a function that transforms a value of some type `a` into another type `b`, an array of values of type `a`, and returns an array of values of type `b`.
```js
// map :: (a -> b) -> [a] -> [b]
let map = f => list =>  list.map(f)
```
