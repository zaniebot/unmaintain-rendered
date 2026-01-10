```yaml
number: 1972
title: "False-positive `unsupported-operator` errors for value-constrained type variable"
type: issue
state: open
author: varchasgopalaswamy
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-17T02:21:09Z
updated_at: 2026-01-08T16:05:44Z
url: https://github.com/astral-sh/ty/issues/1972
synced_at: 2026-01-10T01:56:41Z
```

# False-positive `unsupported-operator` errors for value-constrained type variable

---

_Issue opened by @varchasgopalaswamy on 2025-12-17 02:21_

### Summary

I'm probably doing something wrong, but I can't figure it out. Here's a simple example of what I'm trying to do

```python 
from typing import * 

Scalar = TypeVar("Scalar", int,float)

def test(a:Scalar) -> Scalar: 
    return a * 2

reveal_type(test(10.0)) # should be float 
reveal_type(test(10)) # should be int 
```

[Pyright](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgGoCGIANFCMQG7HkA2A%2BvAsQFBcDKAxi0pQAvIRIUQACgBEAoSBk1UMKsGZhyMAJQ8AJsWCxiAZxhTyALnnNK2qAFoAfFBuVLULlG%2B1iMAK4gKFDkUABUUABMPHSMLOwkUjCm5gCMAAwAdOnaurFMbBzESSlSGblAA) works fine. 

Mypy works, though I can't find a way to share a link to a playground. 

[pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0AqOgB10ogMoBjVFFSU6AXjoAVZjABqcgBTCQUmXN0AaTugZGwUXKgYBKUaMwwwjeAy2pE%2B2ZVt0AtAB8dN5yiCLodFF0lDAMAK6UkaiCdABMDuixAG4wMgD6TMQwWgxuWgCMAAyEVbb2WTC5BUUlZXDu1fUgRiDxDNBwJOSIIADEdACqA1AQTHRg8eiSA7jocJlOLmC8NDb56PE02DCUWvjhrHYBwR2UiKLRMXGJkWC6AHJHJ-d0wPgAX10oh6IDIsUspEIDFoUAoEwACqQIVBSHQ0Fg8Pg6JI1pA2IkbBA1oRRBNxDAYHQABYMBjEOCIAD0TPBzlRhF4bCZMHQTMwuEkcCZuPQ%2BMJqz5C14dFQ2VQ0FQ2FgOLxEAJlCJazouGIkqGojIDGpa38uUocGJkSUugAzIQKhkQCCAb1UCsILkAGLQGAUDE4AjDEAAoA) has no trouble with the operator, but thinks it's a bad return type. However, it's able to correctly infer the return types. 

[ty](https://play.ty.dev/f46e6884-cb9d-438e-a359-fdf38388ddc6) gives an unsupported operator error, and says the type of `test(10.0)` is `float | int`. Is there a more correct way to do what I'm trying to do? 


### Version

_No response_

---

_Comment by @Sillocan on 2025-12-17 04:32_

Using type parameter lists does work in this case.

```python
from typing import * 

Scalar = TypeVar("Scalar", int,float)

def test[Scalar](a:Scalar) -> Scalar: 
    return a * 2

reveal_type(test(10.0)) # should be float 
reveal_type(test(10)) # should be int 
``` 

---

_Label `bug` added by @sharkdp on 2025-12-17 08:34_

---

_Label `generics` added by @sharkdp on 2025-12-17 08:34_

---

_Comment by @sharkdp on 2025-12-17 08:36_

Thank you for reporting this. That is a bug.

The union `float | int` comes from [this special case](https://docs.astral.sh/ty/reference/typing-faq/#why-does-ty-show-int-float-when-i-annotate-something-as-float), but we should not error on the multiplication in the function body.



> Using type parameter lists does work in this case.

That's not equivalent. `test[Scalar](…)` overrides the global `TypeVar` with a new-style (PEP 695) type variable *without any constraints*. The equivalent for the OP example would have to include the TypeVar constraints as well:

```py
def test[Scalar: (int, float)](a:Scalar) -> Scalar: 
    return a * 2

reveal_type(test(10.0))
reveal_type(test(10))
```

And in that case, it demonstrates the same issue.

---

_Added to milestone `Stable` by @sharkdp on 2025-12-17 08:44_

---

_Comment by @varchasgopalaswamy on 2025-12-17 13:58_

Just FYI, same thing happens to add, sub, etc.

---

_Renamed from "Having trouble with a simple generic function" to "False-positive `unsupported-operator` errors for value-constrained type variable" by @AlexWaygood on 2025-12-17 14:06_

---

_Comment by @AlexWaygood on 2025-12-17 14:07_

Here's a minimal repro for the `unsupported-operator` bug:

```py
def f[T: (int, float)](x: T) -> T:
    x + x  # error: [unsupported-operator]
    return x
```

It's not specific to `int`/`float` -- it also repros if the `TypeVar` bounds are `(int, str)`.

---

_Comment by @sharkdp on 2025-12-19 09:43_

Adding a note here that we have an existing test for this with a TODO: https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md?plain=1#L241-L261

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 16:05_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 16:05_

---
