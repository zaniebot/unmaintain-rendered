```yaml
number: 15768
title: "[ty] Bare `ClassVar` annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/bare-classvar-annotation
created_at: 2025-01-27T10:31:21Z
updated_at: 2025-07-08T11:28:17Z
url: https://github.com/astral-sh/ruff/pull/15768
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Bare `ClassVar` annotations

---

_Pull request opened by @sharkdp on 2025-01-27 10:31_

## Summary

It was recently clarified in the [typing spec](https://typing.python.org/en/latest/spec/class-compat.html#classvar) that bare `ClassVar` annotations are allowed. For annotated assignments with a right hand side value, the spec requires type checkers to infer the type as something "to which [the] value is assignable". For a value of `2`, the spec suggests `int`, `Literal[2]`, or `Any` as examples. Here, we choose `Unknown | Literal[2]` instead, conforming with out usual treatment of attribute types.

closes https://github.com/astral-sh/ty/issues/211

---

_Label `red-knot` added by @sharkdp on 2025-01-27 10:31_

---

_Renamed from "[red-knot] Treat bare `ClassVar` annotation as `ClassVar[Unknown]`" to "[red-knot] Bare `ClassVar` annotations" by @sharkdp on 2025-07-07 11:29_

---

_Comment by @github-actions[bot] on 2025-07-07 11:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
-     memo fields = ~17MB
+     memo fields = ~15MB

parso (https://github.com/davidhalter/parso)
-     memo fields = ~54MB
+     memo fields = ~49MB

black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

porcupine (https://github.com/Akuli/porcupine)
-     memo fields = ~106MB
+     memo fields = ~97MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

schemathesis (https://github.com/schemathesis/schemathesis)
-     memo fields = ~129MB
+     memo fields = ~142MB

flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~60MB
+     memo fields = ~66MB

jinja (https://github.com/pallets/jinja)
-     memo fields = ~80MB
+     memo fields = ~88MB

cki-lib (https://gitlab.com/cki-project/cki-lib)
-     memo fields = ~80MB
+     memo fields = ~72MB

rich (https://github.com/Textualize/rich)
-     memo fields = ~106MB
+     memo fields = ~117MB

altair (https://github.com/vega/altair)
-     memo fields = ~228MB
+     memo fields = ~251MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-07-07 11:33_

---

_Review requested from @carljm by @sharkdp on 2025-07-07 11:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-07 11:33_

---

_Review requested from @dcreager by @sharkdp on 2025-07-07 11:33_

---

_Comment by @AlexWaygood on 2025-07-07 12:03_

What's our behaviour for, e.g.

```py
from typing import Final, ClassVar

class Foo:
    X: Final[ClassVar] = 1
```

Do we have any tests for that? Does the spec say anything about it? What are mypy/pyright's behaviour there?

It would be pretty unusual to see something like that since a `Final` declaration in a class body usually implies that it is also a `ClassVar`. But there are a few edge cases, for e.g. dataclasses.

---

_Comment by @AlexWaygood on 2025-07-07 12:05_

Regarding dataclasses: this is what [the spec](https://typing.python.org/en/latest/spec/dataclasses.html) says:

> - `ClassVar` attributes are not considered dataclass fields and are [ignored by dataclass mechanisms](https://docs.python.org/3/library/dataclasses.html#class-variables).
> 
> - A dataclass field may be annotated with `Final[...]`. For example, `x: Final[int]` in a dataclass body specifies a dataclass field `x`, which will be initialized in the generated `__init__` and cannot be assigned to thereafter. A `Final` dataclass field initialized in the class body is not a class attribute unless explicitly annotated with `ClassVar`. For example, `x: Final[int] = 3` is a dataclass field named `x` with a default value of `3` in the generated `__init__` method. A final class variable on a dataclass must be explicitly annotated as e.g. `x: ClassVar[Final[int]] = 3`.

---

_Renamed from "[red-knot] Bare `ClassVar` annotations" to "[ty] Bare `ClassVar` annotations" by @MichaReiser on 2025-07-07 12:09_

---

_Comment by @sharkdp on 2025-07-07 12:25_

> What's our behaviour for, e.g.
> 
> ```python
> from typing import Final, ClassVar
> 
> class Foo:
>     X: Final[ClassVar] = 1
> ```

> What are mypy/pyright's behaviour there?

Mypy infers `Any`. Pyright and pyrefly say that `ClassVar` is not allowed in that context(?).

When we change that to `X: ClassVar[Final] = 1`, Mypy still infers `Any`. And Pyright and pyrefly infer `Literal[1]`.

We infer `Literal[1]` in both cases, which seems reasonable to me. `Final` should take precedence and we should not union with `Unknown` here.

> Do we have any tests for that?

No, added! Thanks.

> Does the spec say anything about it?

According to the [type annotation syntax](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions), both forms should be allowed.

> It would be pretty unusual to see something like that since a `Final` declaration in a class body usually implies that it is also a `ClassVar`

... because you would expect to see the assignment right there in the class body? Right..

> Regarding dataclasses: this is what [the spec](https://typing.python.org/en/latest/spec/dataclasses.html) says:

Interesting! This behavior is certainly not yet implemented, so this will require some more changes. I'll add tests for this (and an implementation, if it's easy) in another PR. It's not really related to the change here.

---

_Comment by @AlexWaygood on 2025-07-07 12:27_

> > Regarding dataclasses: this is what [the spec](https://typing.python.org/en/latest/spec/dataclasses.html) says:
> 
> Interesting! This behavior is certainly not yet implemented, so this will require some more changes. I'll add tests for this (and an implementation, if it's easy) in another PR. It's not really related to the change here.

I know -- but this PR reminded me of it, and it's certainly related to the theme of your work today ;)

---

_@AlexWaygood approved on 2025-07-07 12:30_

---

_Merged by @sharkdp on 2025-07-07 13:04_

---

_Closed by @sharkdp on 2025-07-07 13:04_

---

_Branch deleted on 2025-07-07 13:04_

---

_Comment by @sharkdp on 2025-07-08 11:28_

> > > Regarding dataclasses: this is what [the spec](https://typing.python.org/en/latest/spec/dataclasses.html) says:
> > 
> > 
> > Interesting! This behavior is certainly not yet implemented, so this will require some more changes. I'll add tests for this (and an implementation, if it's easy) in another PR. It's not really related to the change here.
> 
> I know -- but this PR reminded me of it, and it's certainly related to the theme of your work today ;)

I don't know what I read yesterday, but now that I read it again, that all seems very logical — and in fact we already implement everything correctly. Added some tests here: https://github.com/astral-sh/ruff/pull/19202

---
