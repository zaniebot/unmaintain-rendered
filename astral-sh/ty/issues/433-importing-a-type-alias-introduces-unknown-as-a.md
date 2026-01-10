```yaml
number: 433
title: "Importing a type alias introduces `Unknown` as a possible type"
type: issue
state: closed
author: lucach
labels:
  - question
assignees: []
created_at: 2025-05-17T12:20:44Z
updated_at: 2025-10-07T13:49:17Z
url: https://github.com/astral-sh/ty/issues/433
synced_at: 2026-01-10T02:06:24Z
```

# Importing a type alias introduces `Unknown` as a possible type

---

_Issue opened by @lucach on 2025-05-17 12:20_

### Summary

On the following code, ty correctly reports that the argument to function `f` is incorrect ([playground](https://play.ty.dev/a8e61631-a91e-4b20-a518-936db98fb4e1)):

```py
class A: pass
B = A
def f(b: B): pass
f("")  # Argument to function `f` is incorrect: Expected `A`, found `Literal[""]` (invalid-argument-type) [Ln 4, Col 3]
```

When the last two lines are moved into a separate file, and B is imported, ty no longer detects the error ([playground](https://play.ty.dev/b140e655-bd05-4de5-af85-140fca15543b)):

```py
from main import B
def f(b: B): pass
f("")
```

In the main file, the implicit type alias `B` is correctly inferred to be `<class 'A'>`. But when it's imported from another module, ty infers `Unknown | <class 'A'>`.

I'm not sure how broad the actual underlying issue is.

### Version

_No response_

---

_Label `question` added by @sharkdp on 2025-05-17 18:37_

---

_Comment by @sharkdp on 2025-05-17 18:52_

Thank you for your report. We know that this behavior deviates from other type checkers, but it is intentional. The rationale is explained in [this document](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md) (and more information [here](https://github.com/astral-sh/ruff/blob/b86960f18c010ef991bc91e93bcff84b44a539fd/crates/ty_python_semantic/resources/mdtest/boundness_declaredness/public.md)).

The TLDR is: if you refer to a symbol from another scope, we use the "public type" of that symbol. For *undeclared* symbols, we union with `Unkown`. If you want to avoid this, you need to declare `B`.

Declaring `B` is not straightforward in this example, because there's no way to spell out the type of `A`, which is a special singleton type that only has one inhabitant: the class A (it's possible using `ty_extensions.TypeOf[A]`, but that is not a recommended/portable solution). What you could do instead is to use a type alias (`type B = A`), which [works fine](https://play.ty.dev/b08b21d2-c406-45a8-ab60-22c374e968a2) for this case.

---

_Comment by @AlexWaygood on 2025-05-17 21:19_

When we support `typing(_extensions).Final` and/or `typing(_extensions).TypeAlias`, it would also be plausible to annotate `B` using either of those symbols. (We don't support either yet, but we hopefully will soon.)

---

_Comment by @lucach on 2025-05-19 05:53_

Thank you for the detailed explanation. This example is actually a minimal reproduction of a concrete situation I have with a library I am maintaining. Using the [type statement](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement) would be a great solution, but that has been introduced in Python 3.12 and at least for I while I can't drop the compatibility with Python 3.10. Older Python versions generate a syntax error for that construct.

I guess a reasonable solution would be using the `TypeAlias` annotation, as @AlexWaygood suggested. Looking forward to that! :-)

Thanks again for the work and the replies.     

---

_Comment by @karlicoss on 2025-06-25 21:45_

I have a simpler example which I didn't expect to infer `Unknown`:
https://play.ty.dev/d33d77c8-ff6b-48cc-8b4e-11d56133ebbb
```
# main.py
from module import number
reveal_type(number)
```
```
# module.py
number = 123
```
This results in `Revealed type: Unknown | Literal[123] (revealed-type) [Ln 2, Col 13]`


I read https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md, and:

- I understand it's a deliberate choice by `uv`? Is the idea that you are meant to always declare the type for a 'public' module attribute? (this is different from mypy where it would infer `int`)
- Perhaps the documentation in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md could be updated -- from reading it I thought that the issue is more with class attributes. It's more customary to explicitly annotate class attributes rather than module attributes in my experience, although in hindsight it is kind of the same thing.
- Is there practical implication of having `Unknown | <DeclaredType>` in imported symbols if I'm not reassigning then? Everything would still be type checked correctly as if the type was just `<DeclaredType`? E.g. that can often be the case in existing third party libraries which don't annotate global module attributes.

Thanks!

Possibly relevant: https://github.com/astral-sh/ty/issues/622

---

_Comment by @karlicoss on 2025-06-26 21:58_

Another oddity I ran into which is kinda relevant
https://play.ty.dev/42bd249d-a02c-4190-9813-7cd09b6cd948
```python
from typing import Never, reveal_type

def never() -> Never:
    raise RuntimeError()

class U:
    unknown_or_literal = 123
    unknown_or_never = never()

reveal_type(U.unknown_or_literal)
reveal_type(U.unknown_or_never)
```
```
    Revealed type: `Unknown | Literal[123]` (revealed-type) [Ln 10, Col 13]
    Revealed type: `Unknown` (revealed-type) [Ln 11, Col 13]
```
So seems like ty thinks that `Unknown | Never = Unknown` -- I guess this makes sense from type theory perspective, but in this case it's a bit of a gotcha, perhaps.




The example above is a bit contrived, but I ran into it while making `pytest.skip` function compatible with `ty`. Simplified, it's something like this:
```python
from typing import Never, reveal_type

def never() -> Never:
    raise RuntimeError()

class pytest:
    skip = never

reveal_type(pytest.skip())
```
, which reveals type as `Unknown` instead of `Never` as one might expect.


---

_Comment by @carljm on 2025-06-27 00:01_

Yes, this is an intentional choice; if you don't declare a type for an attribute, we assume anything can be assigned to that attribute, so we know a "lower bound" for the type (what was assigned) but we don't know any upper bound (thus the union with `Unknown`). If you use that attribute in any way that doesn't work with the known lower bound, you'll still get a type error. And yes, this applies to both module and class attributes the same. If you don't want the union with `Unknown`, the fix is to annotate the attribute.

---

_Comment by @sharkdp on 2025-07-21 18:38_

> When we support `typing(_extensions).Final` and/or `typing(_extensions).TypeAlias`, it would also be plausible to annotate `B` using either of those symbols. (We don't support either yet, but we hopefully will soon.)

`typing.Final` is now supported, so this can be used as a workaround until we also support `typing.TypeAlias`: The error is correctly emitted here: https://play.ty.dev/217804c3-810b-4e19-9744-ef1229d787d4

---

_Comment by @AlexWaygood on 2025-10-06 16:28_

The original snippet in this issue no longer provokes the bug here, following the changes made in https://github.com/astral-sh/ruff/pull/20664. We're still working on `typing.TypeAlias` support, but that's tracked in https://github.com/astral-sh/ty/issues/544. So I think this can be closed now.

Thanks for the report @lucach!

---

_Closed by @AlexWaygood on 2025-10-06 16:28_

---

_Comment by @lucach on 2025-10-07 13:49_

I can confirm this solved the problem in the larger project from which this minimal reproduction was created. Thank you for all the work!

---
