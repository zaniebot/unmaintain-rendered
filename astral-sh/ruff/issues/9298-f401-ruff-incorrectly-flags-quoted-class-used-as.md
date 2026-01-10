---
number: 9298
title: "F401: Ruff Incorrectly Flags Quoted Class used as Generic Parameter as Unused"
type: issue
state: open
author: max-muoto
labels:
  - bug
  - type-inference
  - linter
assignees: []
created_at: 2023-12-28T06:48:40Z
updated_at: 2025-04-26T12:35:44Z
url: https://github.com/astral-sh/ruff/issues/9298
synced_at: 2026-01-10T01:22:49Z
---

# F401: Ruff Incorrectly Flags Quoted Class used as Generic Parameter as Unused

---

_Issue opened by @max-muoto on 2023-12-28 06:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


To give some context, I have `from __future__ import annotations` required in every-single file, and the TCH rule-set enabled across my codebase, along with the full Pyflakes rule-set.

I'm running into a situation like this:

```python
from __future__ import annotations

from typing import TypeVar, Generic
from my_module import ModelB

T = TypeVar("T")

class BaseModel(Generic[T]):
   ...

class ModelA(BaseClass[ModelB]]
   ...
```

Here, even though `ModelB` is only being passed in as the generic parameter, TCH moves it out of the type-checking block. If I try and quote `ModelB`.
 

```python
from __future__ import annotations

from typing import TypeVar, Generic, TYPE_CHECKING

if TYPE_CHECKING:
    from my_module import ModelB  # Ruff flags `ModelB` as unused.

T = TypeVar("T")

class BaseModel(Generic[T]):
   ...

class ModelA(BaseClass["ModelB"]]
   ...
```

In this case my type-checker (Pyright) still works as expected, however [`F401`](https://docs.astral.sh/ruff/rules/unused-import/) flags  `ModelB` as unused and removes it. Unfortunately in my situtation, importing `ModelB` causes a circular import, so it would need it to go under the `TYPE_CHECKING` block . As a note I also have [`UP037`](https://docs.astral.sh/ruff/rules/quoted-annotation/) enabled, and it doesn't attempt to remove the quotes in this case.

Lot of intertwining rules here, so happy to clarify anything about my configuration. Also, I'm not sure if TCH moving `ModelB` when non-quoted out of the `TYPE_CHECKING` block is also a bug.

Ruff Version: 0.1.9
Ruff Command: `ruff check .`

---

_Comment by @charliermarsh on 2023-12-28 13:01_

The general problem here is that, when looking at `BaseClass[ModelB]`, we don't have a great way to know whether `[Model]` is a generic annotation or an actual subscript access (like `BASE_CLASSES[0]`, as a counterexample). We could come up with some better heuristics, but right now, we effectively scope generics to those that are known in the standard library (like `Sequence`).

We do have a mechanism for marking classes as generics: [`extend-generics`](https://docs.astral.sh/ruff/settings/#pyflakes-extend-generics). Maybe that can help here?


---

_Label `bug` added by @charliermarsh on 2023-12-31 12:38_

---

_Comment by @max-muoto on 2024-01-01 17:51_

> The general problem here is that, when looking at `BaseClass[ModelB]`, we don't have a great way to know whether `[Model]` is a generic annotation or an actual subscript access (like `BASE_CLASSES[0]`, as a counterexample). We could come up with some better heuristics, but right now, we effectively scope generics to those that are known in the standard library (like `Sequence`).
> 
> We do have a mechanism for marking classes as generics: [`extend-generics`](https://docs.astral.sh/ruff/settings/#pyflakes-extend-generics). Maybe that can help here?

Makes sense on why this is then! 

Yes, that actually solves my problem, I didn't know there was a configuration setting for this. Really appreciate it!

---

_Comment by @max-muoto on 2024-01-01 17:55_

I'm assuming not, but does it support wildcards? For example, if you wanted to treat all of your Django models as valid generics you might want to do `extend-generics = ["src.my_app.models.*"]`

---

_Comment by @charliermarsh on 2024-01-02 18:24_

I don't believe so... but we could extend it to support wildcards.

---

_Comment by @M1troll on 2024-01-24 08:56_

Hi!

I have the same problem with generic annotations.
I read suggestions above and I think the "store all **unused** imports in settings" way is a bit odd

---

_Referenced in [astral-sh/ruff#10676](../../astral-sh/ruff/issues/10676.md) on 2024-03-30 23:39_

---

_Renamed from "Ruff Incorrectly Flags Quoted Class used as Generic Parameter as Unused" to "F401: Ruff Incorrectly Flags Quoted Class used as Generic Parameter as Unused" by @AlexWaygood on 2024-03-30 23:46_

---

_Label `linter` added by @AlexWaygood on 2024-03-30 23:46_

---

_Comment by @sterliakov on 2024-03-30 23:49_

`extend-generics` is a good solution for general case. But... Would the heuristic "subscript expression in parent classes list" work? I agree that it can cause a false positive (below), but such case would be marginally less popular, I guess (I do not remember ever inheriting from dict lookup example), and can be easily overcome without linter silencing (below-2).

So, I suggest to treat any string that is a valid identifier as a name when used in a subscript in class parents list.

FP example:

```python
BASES = {"foo": int, "bar": str}
class A(Bases["foo"]): ...  # Name "foo" not found
```

To overcome:
```python
BASES = {"foo": int, "bar": str}
A_BASE = BASES["foo"]
class A(A_BASE): ...
```

We may also want to extend this a bit, so that `"name"` should be interpreted as a name:

* `class X(Foo["name"])`
* `X: TypeAlias = Foo["name"]` and right-hand side of new 3.12 `type` expression
* Anything else I'm missing?

Alternatively, this may be introduced as some kind of "soft name" that may be absent from namespace, but marks the name as "used" in scope. With such approach the FP example above is no longer failing, and I do not see any backwards-incompatible consequences from doing that.

---

_Referenced in [astral-sh/ruff#13181](../../astral-sh/ruff/issues/13181.md) on 2024-09-01 18:24_

---

_Referenced in [astral-sh/ruff#15560](../../astral-sh/ruff/issues/15560.md) on 2025-01-17 22:17_

---

_Referenced in [astral-sh/ruff#15860](../../astral-sh/ruff/issues/15860.md) on 2025-02-01 17:05_

---

_Referenced in [astral-sh/ruff#12780](../../astral-sh/ruff/issues/12780.md) on 2025-02-01 17:06_

---

_Label `type-inference` added by @AlexWaygood on 2025-02-01 17:07_

---

_Referenced in [astral-sh/ruff#17455](../../astral-sh/ruff/issues/17455.md) on 2025-04-26 12:16_

---

_Comment by @pekkaklarck on 2025-04-26 12:35_

I encountered this with our project as well. Using `extend-generics` fixed it, but, as commented above, feels like a hack. Considering the amount of duplicates and a similar problem with TypeAlias (#17455), I believe it would be good to have some kind of a fix for this even though it wouldn't be perfect.

---

_Referenced in [aiidateam/aiida-core#6704](../../aiidateam/aiida-core/pulls/6704.md) on 2025-05-06 02:27_

---

_Referenced in [aiidateam/aiida-core#7019](../../aiidateam/aiida-core/pulls/7019.md) on 2025-09-25 17:06_

---
