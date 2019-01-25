# Wunderkammer

Curios from foreign lands. Mostly me stealing from Haskell to bring to JavaScript, promethean-like.

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
Function.prototype.map = function(fn) {
  return (...args) => fn(this(...args))
}

// Contravariant
Function.prototype.contramap = function(fn) {
  return (...args) => this(fn(...args))
}
```
