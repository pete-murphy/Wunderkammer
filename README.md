# Wunderkammer

Mostly stealing from Haskell.

### Password generator?
This one is based off a Mark Seeman blog post.
He gives examples in Haskell (using `sprintf` magic).
The cool thing is that in JavaScript we have _n-ary_ (i.e., _variadic_ or _arbitrary arity_) functions. So this works:

```js
import { liftN, map, sequence } from "ramda"

liftN(6, (...args) => args.join(""))(
  ["P", "p"],
  ["a", "4"],
  ["ssw"],
  ["o", "0"],
  ["rd"],
  ["", "!"]
)

// Pretty sure this returns the same thing:
map(xs => xs.join(""))(
  sequence(Array.of, [
    ["P", "p"],
    ["a", "4"],
    ["ssw"],
    ["o", "0"],
    ["rd"],
    ["", "!"]
  ])
)
```

### The `Function` functor

```js
// Covariant
// map :: (r -> a) ~> (a -> b) -> (r -> b)
Function.prototype.map = function(fn) {
  return (...args) => fn(this(...args))
}

// Contravariant
// contramap :: (r -> a) ~> (r -> s) -> (s -> a)
Function.prototype.contramap = function(fn) {
  return (...args) => this(fn(...args))
}

// Applicative
// ap :: (r -> a -> b) ~> (r -> a) -> (r -> b)
Function.prototype.ap = function(ra) {
  return (...r) => this(...r)(ra(...r))
}
```

A monad instance would be more useful, I think.

### Recursive `fold`

Not optimal

```js
const fold = (foldingFn, acc) => ([x, ...xs]) =>
  acc !== undefined
    ? x === undefined
      ? acc
      : fold(foldingFn, foldingFn(acc, x))(xs)
    : fold(foldingFn, x)(xs)

const sum = fold((acc, x) => x + acc)
const reverse = fold((acc, x) => [x].concat(acc))
const any = predFn =>
  fold((acc, x) => predFn(x) || acc, false)
```

### Array monad and applicative
```js
// concatMap :: (a -> [b]) -> [a] -> [b]
const concatMap = f => ([x, ...xs], acc = []) =>
  x === undefined
    ? acc
    : concatMap(f)(xs, [...acc, ...f(x)])

// ap :: ([a -> b]) -> [a] -> [b]
const ap = fs => xs => concatMap(f => xs.map(x => f(x)))(fs)
```
