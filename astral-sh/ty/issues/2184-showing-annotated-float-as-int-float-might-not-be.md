```yaml
number: 2184
title: "Showing annotated `float` as `int | float` might not be right"
type: issue
state: open
author: cmp0xff
labels:
  - needs-decision
assignees: []
created_at: 2025-12-23T12:26:43Z
updated_at: 2025-12-29T22:07:21Z
url: https://github.com/astral-sh/ty/issues/2184
synced_at: 2026-01-12T15:54:26Z
```

# Showing annotated `float` as `int | float` might not be right

---

_@cmp0xff_

### Summary

- [`Y041` in `flake8`](https://pypi.org/project/flake8-pyi/22.7.0/) marks `float | int` as redundant
- [PEP484](https://peps.python.org/pep-0484/#the-numeric-tower) suggests `float` contains `int`
- `mypy`, `pyright` and [`pyrefly`](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2UYANxiooAfSbEYAHSwwwdMLlwAKAJR0AtAD4lUXKgaI5dMwJgMArpXR0AjHLmCRYycxirla9erkgAGhArBmg4EnJEEABiOgBVUKgIJiUrdABjUNx0OCd5RWVKGiNxdCsabBhKVXxETnQGTV06OAZKEztzQWtbJRkQADlyyva6YHwAX37-ILJBMChSQgZaKApYgAVSecWWjBwCOnTsyDYbIwhswjlYgGUYGDoACwYGYjhEAHpPuYVFwl4bE%2BMHQn0wuHScE%2Bx3Qp3OWVBSl4dFQQlQ0FQ2FgRxOEDOlAu2TouGICPCcjIDCe2S0IkocEudgAvHR%2BgBmQj2ABM03QIAmQVQmQgIgAYtAYBQ0Fg8EQyPygA) show annotated `float` as `float`
- However, [`ty`](https://play.ty.dev/6d776ddf-569d-4a56-b8b1-2c2d73166635) shows annotated `float` as `int | float`

```py
from typing import reveal_type
def foo() -> float:
    return 1

reveal_type(foo())  # ty shows int | float
```

### Version

5ea30c4c5

---

_Comment by @AlexWaygood on 2025-12-23 12:29_

Hey, thanks for the report. Does this FAQ answer your question? https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-show-int-float-when-i-annotate-something-as-float

---

_Label `question` added by @AlexWaygood on 2025-12-23 12:29_

---

_Comment by @cmp0xff on 2025-12-23 12:39_

Hi, thank you for showing me the FAQ. What I wanted to say is that `ty` goes against a PEP here, which can be surprising to users.

---

_Comment by @carljm on 2025-12-23 16:13_

I don't think ty "goes against a PEP" here. Interpreting an annotation of `float` as meaning `float | int` is one way to implement the (not very well specified) comment in PEP 484 about an int "being acceptable" where `float` is annotated. It is also consistent with `Y041` -- an annotation of `float | int` is redundant when `float` already means `float | int`.

`float` and `int` are different (and not compatible) types at runtime, so the choice in PEP 484 to pretend they are compatible has left type-checkers with not a great set of choices here. The implementation in ty is not so different from that in pyright, except we choose to show the type to users in the same way it is understood internally.

---

_Closed by @carljm on 2025-12-23 16:13_

---

_Comment by @carljm on 2025-12-26 17:28_

I'm going to reopen this issue to track whether we do need to do something differently to reduce user confusion here, because we get a lot of reports around this.

Options could include:
1) Choosing a different (and unsound) interpretation of the int/float special case.
2) Displaying the `int | float` union type differently.

---

_Reopened by @carljm on 2025-12-26 17:28_

---

_Label `question` removed by @carljm on 2025-12-26 17:28_

---

_Label `needs-decision` added by @carljm on 2025-12-26 17:28_

---

_Added to milestone `Stable` by @carljm on 2025-12-26 17:28_

---

_Comment by @cmp0xff on 2025-12-26 22:33_

Thank you for opening the Issue again. Based on my understanding in pandas-dev/pandas-stubs#1574, I think printing as JustFloat | JustInt makes a lot of sense.

---

_Comment by @Jammf on 2025-12-27 23:58_

I would argue that the special-casing in the typing spec only applies to **argument type** annotations. This seems to differ from ty's interpretation, which also treats explicit **return type** `float` annotations as `float | int`.

From [Python typing spec](https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex), emphasis added:
> Python’s numeric types complex, float and int are not subtypes of each other, but to support common use cases, the type system contains a straightforward shortcut: when an **argument** is annotated as having type float, an argument of type int is acceptable; similar, for an **argument** annotated as having type complex, arguments of type float or int are acceptable.

If only arguments have special-casing, that would imply that the following are typed incorrectly and should each emit a diagnostic:

```python
def foo() -> float:
    return 1

def bar(a: float) -> float:   # equivalently: (float | int) -> JustFloat
    return a

def baz(a: float, b: float) -> float:   # equivalently: (float | int, float | int) -> JustFloat
    return a + b
````

since `bar(1)` and `baz(1, 2)` both return  an `int`, which is incompatible with `float`. Some potential fixes that users could do, depending on their intent:

```python
# Modify annotations to clarify expected types
def foo() -> int: ...

def bar(a: float) -> float | int: ...
# -or-
def bar(a: JustFloat) -> float: ...

def baz(a: float, b: float) -> float | int: ...
# -or-
def baz(a: JustFloat, b: JustFloat) -> float: ...

# ---

# Modify impl. to match annotations
def foo() -> float:
    return float(1)

def bar(a: float) -> float:
    return float(a)

def baz(a: float, b: float) -> float:
    return float(a + b)
````

---

_Comment by @carljm on 2025-12-29 20:09_

It is true that the spec only mentions arguments, but no type checker has ever interpreted the special case in that way (all type checkers are OK with your first `foo` function). I would attribute the wording in the spec to careless word choice in PEP 484 rather than a real intention that the special case should apply only to arguments, given that the authors of PEP 484 were also among the original authors of mypy.

We could of course decide to follow the literal wording in the spec, but since this would be a change from the behavior of all current type checkers, we should not do it just "because the spec says so," but only if we actually believe it's a better approach. I definitely don't think it is. IMO it is a non-starter to have your `bar` function fail to type-check.

There is general agreement already that the current wording in the spec is under-specified and does not clearly describe a desirable behavior that type checkers are willing to implement. As a result, every type checker currently implements a slightly different (and differently confusing) version of the special case. There isn't, however, consensus yet on how best to clarify it. See https://github.com/python/typing/issues/1746 for extensive prior discussion.

---

_Comment by @Jammf on 2025-12-29 21:54_

Thank you for the link to the prior discussion, it seems there has been quite an extensive debate! It definitely does sound like a clarification to the spec is needed. The reason I commented earlier was because of the following scenario, which I assumed to be an oversight rather than intentional design:

```python
a = 0.1
b = 0.2
reveal_type(a)  # float
reveal_type(b)  # float

c = a + b
reveal_type(c)  # int | float
```

I had expected the result of adding two `JustFloat`s would be another `JustFloat`, and figured the `float.__add__(self, value: float, /) -> float` declaration from typeshed was causing ty to incorrectly determine the return type was `int | float`. I opted to add on to this issue since the root causes overlapped, but I'm happy to raise a separate issue if one doesn't already exist.

---

_Comment by @carljm on 2025-12-29 22:06_

Makes sense! Feel free to open another issue for that, though I'm not sure there is an obvious short-term resolution in ty, pending a clarification in the spec. If we can agree on the "`float` annotation means `int | float`" interpretation of the special case, which ty uses, then it would make sense to introduce a `JustFloat` annotation, and it would make sense for typeshed to use that type in the return type of `float.__add__`. But it will be difficult to make this change in typeshed before we have agreement on how type-checkers should handle `float` annotations.

We could consider patching typeshed or special-casing some methods in ty as a short-term fix.

---
