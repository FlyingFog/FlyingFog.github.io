## Primitive Types and Values

The *primitive* types are the built-in and most basic types: integers, floating-point numbers, characters, strings, and booleans. They will be recognizable as similar to primitive types from other programming languages.

**Type `int`: Integers.** OCaml integers are written as usual: `1`, `2`, etc. The usual operators are available: `+`, `-`, `*`, `/`, and `mod`. The latter two are integer division and modulus:

**Type `float`: Floating-point numbers.** OCaml floats are [IEEE 754 double-precision floating-point numbers](https://en.wikipedia.org/wiki/Double-precision_floating-point_format). Syntactically, they must always contain a dot—for example, `3.14` or `3.0` or even `3.`. The last is a `float`; if you write it as `3`, it is instead an `int`:

The same behavior can be observed in Python and Java, too. If you haven’t encountered this phenomenon before, here’s a [basic guide to floating-point representation](https://floating-point-gui.de/basic/) that you might enjoy reading.

**Type `bool`: Booleans.** The boolean values are written `true` and `false`. The usual short-circuit conjunction `&&` and disjunction `||` operators are available.

**Type `char`: Characters.** Characters are written with single quotes, such as `'a'`, `'b`, and `'c'`. They are represented as bytes —that is, 8-bit integers— in the ISO 9958-1 “Latin-1” encoding. The first half of the characters in that range are the standard ASCII characters. You can convert characters to and from integers with `char_of_int` and `int_of_char`.

**Type `string`: Strings.** Strings are sequences of characters. They are written with double quotes, such as `"abc"`. The string concatenation operator is `^`:

**Syntax.** The syntax of an `if` expression:

```
if e1 then e2 else e3
```

The letter `e` is used here to represent any other OCaml expression; it’s an example of a *syntactic variable* aka *metavariable*, which is not actually a variable in the OCaml language itself, but instead a name for a certain syntactic construct. The numbers after the letter `e` are being used to distinguish the three different occurrences of it.

**Dynamic semantics.** The dynamic semantics of an `if` expression:

- If `e1` evaluates to `true`, and if `e2` evaluates to a value `v`, then `if e1 then e2 else e3` evaluates to `v`
- If `e1` evaluates to `false`, and if `e3` evaluates to a value `v`, then `if e1 then e2 else e3` evaluates to `v`.

We call these *evaluation rules*: they define how to evaluate expressions. Note how it takes two rules to describe the evaluation of an `if` expression, one for when the guard is true, and one for when the guard is false. The letter `v` is used here to represent any OCaml value; it’s another example of a metavariable. Later we will develop a more mathematical way of expressing dynamic semantics, but for now we’ll stick with this more informal style of explanation.

**Static semantics.** The static semantics of an `if` expression:

- If `e1` has type `bool` and `e2` has type `t` and `e3` has type `t` then `if e1 then e2 else e3` has type `t`

We call this a *typing rule*: it describes how to type check an expression. Note how it only takes one rule to describe the type checking of an `if` expression. At compile time, when type checking is done, it makes no difference whether the guard is true or false; in fact, there’s no way for the compiler to know what value the guard will have at run time. The letter `t` here is used to represent any OCaml type; the OCaml manual also has definition of [all types](https://ocaml.org/manual/types.html) (which curiously does not name the base types of the language like `int` and `bool`).

**Syntax.**

```
let x = e1 in e2
```

As usual, `x` is an identifier. These identifiers must begin with lower-case, not upper, and idiomatically are written with `snake_case` not `camelCase`. We call `e1` the *binding expression*, because it’s what’s being bound to `x`; and we call `e2` the *body expression*, because that’s the body of code in which the binding will be in scope.

**Dynamic semantics.**

To evaluate `let x = e1 in e2`:

- Evaluate `e1` to a value `v1`.
- Substitute `v1` for `x` in `e2`, yielding a new expression `e2'`.
- Evaluate `e2'` to a value `v2`.
- The result of evaluating the let expression is `v2`.

Here’s an example:

```
    let x = 1 + 4 in x * 3
-->   (evaluate e1 to a value v1)
    let x = 5 in x * 3
-->   (substitute v1 for x in e2, yielding e2')
    5 * 3
-->   (evaluate e2' to v2)
    15
      (result of evaluation is v2)
```

**Static semantics.**

- If `e1 : t1` and if under the assumption that `x : t1` it holds that `e2 : t2`, then `(let x = e1 in e2) : t2`.

We use the parentheses above just for clarity. As usual, the compiler’s type inferencer determines what the type of the variable is, or the programmer could explicitly annotate it with this syntax:

```
let x : t = e1 in e2
```