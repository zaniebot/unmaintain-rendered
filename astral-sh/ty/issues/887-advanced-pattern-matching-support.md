```yaml
number: 887
title: Advanced pattern matching support
type: issue
state: open
author: sharkdp
labels:
  - narrowing
  - control flow
assignees: []
created_at: 2025-07-25T07:11:57Z
updated_at: 2025-12-18T08:43:24Z
url: https://github.com/astral-sh/ty/issues/887
synced_at: 2026-01-12T15:54:24Z
```

# Advanced pattern matching support

---

_@sharkdp_

We already have basic support for control flow analysis and type narrowing in `match` statements, but our support is not complete. The list below shows some examples of things that are not working yet, but is probably not exhaustive:

* [ ] Type inference for names that are bound in patterns:
  ```py
  from dataclasses import dataclass
  
  @dataclass
  class Person:
      name: str
      age: int
  
  def f(person: Person):
      match person:
          case Person(name, age):
              reveal_type(name)  # @Todo(`match` pattern definition types)
              reveal_type(age)  # @Todo(`match` pattern definition types)
  
  def g(xs: list[int]):
      match xs:
          case [1, 2, x]:
              reveal_type(x)  # @Todo(`match` pattern definition types)
  ```

* [ ] Support for `__match_args__`
* [ ] [Matching on builtin classes](https://docs.python.org/3/reference/compound_stmts.html#class-patterns)

---

_Label `narrowing` added by @sharkdp on 2025-07-25 07:11_

---

_Label `control flow` added by @sharkdp on 2025-07-25 07:11_

---

_Added to milestone `GA` by @carljm on 2025-07-25 17:28_

---

_Comment by @UnboundVariable on 2025-07-25 21:14_

Here are some additional cases to track:
* [ ] Support for `Callable()` in class patterns
```python
def test(subj: int | type[int] | Callable[..., str]) -> int | str | None:
    match subj:
        case Callable():
            reveal_type(subj)  # type[int] | ((...) -> str)

        case _:
            reveal_type(subj)  # int
```
 
* [ ] `self` as a subject expression:
```python
class ClassA:
    def method1(self) -> str:
        match self:
            case ClassA():
                return ""
```

* [ ] Exhaustion detection for sequence patterns with a star entry:
```python
def test() -> int:
    match [10]:
        case [*values]:
            return values[0]
```

* [ ] Inferring types from literal patterns:
```python
def test(subj):
    match subj:
        case 3 as a1, -3 as a2:
            reveal_type(a1) # Literal[3]
            reveal_type(a2) # Literal[-3]
```

* [ ] Inferring sequence type from sequence pattern:
```python
def test(subj):
    match subj:
        case a, b:
            reveal_type(subj)  # Sequence[Unknown]

        case 1, 2:
            reveal_type(subj)  # Sequence[int]
```

* [ ] Narrowing for `None`:
```python
def test(subj: int | None):
    match subj:
        case None as a:
            reveal_type(a) # None

        case b:
            reveal_type(b) # int
```

* [ ] Discriminated union narrowing based on literal attribute types:
```python
class A:
    tag: Literal["a"]
    name: str


class B:
    tag: Literal["b"]
    num: int


def test(subj: A | B) -> None:
    match subj.tag:
        case "c":
            reveal_type(subj)  # Never

        case "a":
            reveal_type(subj)  # A

        case _:
            reveal_type(subj)  # B
```

* [ ] Narrowing based on mapping pattern:
```python
def test(subj: int | dict[str, str]):
    match subj:
        case {1: _}:
            reveal_type(subj)  # Never

        case {"": ""}:
            reveal_type(subj)  # dict[str, str]
```

* [ ] Discriminated TypedDict narrowing based on mapping pattern:
```python
class IntValue(TypedDict):
    type: Literal["Int"]
    value: int

class StrValue(TypedDict):
    type: Literal["Str"]
    value: str

def test(subj: IntValue | StrValue | int) -> None:
    match subj:
        case {"type": "Int"}:
            reveal_type(subj)  # IntValue

        case {"type": "Str"}:
            reveal_type(subj)  # StrValue

        case _:
            reveal_type(subj)  # int
```

* [ ] Tuple subject expressions with tuple expansion:
```python
class A1: pass
class A2: pass
class B1: pass
class B2: pass

type A = A1 | A2
type B = B1 | B2

def test(a: A, b: B):
    subj = a, b
    match subj:
        case A1(), B1():
            reveal_type(subj)  # tuple[A1, B1]

        case A1(), _:
            reveal_type(subj)  # tuple[A1, B2]

        case _:
            reveal_type(subj)  # tuple[A2, B1 | B2]
```

* [ ] Length-based narrowing of tuple subjects:
```python
def test(subj: tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], complex]):
    match subj:
        case (x,):
            reveal_type(subj)  # tuple[int]

        case (x, y):
            reveal_type(subj)  # tuple[str, str] | tuple[int, complex]

        case (x, y, z):
            reveal_type(subj)  # tuple[int, str, complex]
```

* [ ] Narrowing enums based on value patterns:
```python
class Color(Enum):
    Red = "red"
    Blue = "blue"

class Pet(Enum):
    Dog = "dog"
    Cat = "cat"

def test(subj: Color | Pet):
    match subj:
        case Color.Red | Color.Blue:
            reveal_type(subj)  # Color

        case Pet.Dog:
            reveal_type(subj)  # Pet.Dog

        case _:
            reveal_type(subj)  # Pet.Cat
```

* [ ] Value patterns using `Final` values:
```python
class Numbers:
    ZERO: Final = 0.0
    ONE: Final = 1
    INFINITY: Final = float("inf")

def test(subj: float):
    match subj:
        case Numbers.ONE:
            reveal_type(subj)  # Literal[1]

        case Numbers.INFINITY:
            reveal_type(subj)  # float

        case Numbers.ZERO:
            reveal_type(subj)  # float

        case _:
            reveal_type(subj)  # float
```

* [ ] Narrowing of expressions used in tuple subject expression:
```python
def test(a: int, b: str):
    match a, b:
        case 1, "hi":
            reveal_type(a)  # Literal[1]
            reveal_type(b)  # Literal["hi"]
```

---

_Comment by @silamon on 2025-08-29 09:03_

I've been looking a bit into how to code the remaining bullets: 
1) It's missing the `match` pattern definition types in infer, but I'm not seeing how we are able to set up an expression there using infer_expression_types. The code comment <https://github.com/astral-sh/ruff/blob/0d7ed32494df58f62ed6b48a3491f258ca7fd086/crates/ty_python_semantic/src/types/infer.rs#L3664> there was maybe too hard to understand for me. 
2) The match examples rely mostly on sequences and some even more advanced matching examples, but looks like they are missing here:  <https://github.com/astral-sh/ruff/blob/0d7ed32494df58f62ed6b48a3491f258ca7fd086/crates/ty_python_semantic/src/semantic_index/builder.rs#L816> to start with. It feels to me that it was left behind because it needs more refactoring to handle multiple assignments.

Anyway, anybody who can give some code pointers on how to handle these or what the challenges could be? 


---

_Comment by @sharkdp on 2025-09-01 08:29_

> It's missing the `match` pattern definition types in infer, but I'm not seeing how we are able to set up an expression there using infer_expression_types. The code comment https://github.com/astral-sh/ruff/blob/0d7ed32494df58f62ed6b48a3491f258ca7fd086/crates/ty_python_semantic/src/types/infer.rs#L3664 there was maybe too hard to understand for me.

@dhruvmanila This was written by you originally, maybe you can have a look?

> The match examples rely mostly on sequences and some even more advanced matching examples, but looks like they are missing here: https://github.com/astral-sh/ruff/blob/0d7ed32494df58f62ed6b48a3491f258ca7fd086/crates/ty_python_semantic/src/semantic_index/builder.rs#L816 to start with. It feels to me that it was left behind because it needs more refactoring to handle multiple assignments.

That's certainly possible, but I don't think it hasn't been implemented because there are any blocking issues. We do handle multiple assignments in other places (e.g. unpacking). It's just that no-one has attempted to implement any of these advanced pattern matching features, so far. 

---

_Comment by @silamon on 2025-09-01 16:08_

The unpackables make it easier to work all this, but for some reason the matching uses an index based system: 

```rust
#[derive(Debug, PartialEq)]
struct CurrentMatchCase<'ast> {
    /// The pattern that's part of the current match case.
    pattern: &'ast ast::Pattern,

    /// The index of the sub-pattern that's being currently visited within the pattern.
    ///
    /// For example:
    /// ```py
    /// match subject:
    ///     case a as b: ...
    ///     case [a, b]: ...
    ///     case a | b: ...
    /// ```
    ///
    /// In all of the above cases, the index would be 0 for `a` and 1 for `b`.
    index: u32,
}
```

Reading the code, it would make very much sense to make the match ast unpackable as well.

---

_Comment by @flying-sheep on 2025-12-18 08:29_

You can probably add #561 as a sub-issue of this one.

---
