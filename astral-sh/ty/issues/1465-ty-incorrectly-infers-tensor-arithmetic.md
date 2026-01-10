```yaml
number: 1465
title: "ty incorrectly infers tensor arithmetic expression as `int | float` instead of `Tensor`"
type: issue
state: closed
author: anmorgunov
labels:
  - bug
  - type-inference
assignees: []
created_at: 2025-11-02T15:02:05Z
updated_at: 2025-11-12T01:27:57Z
url: https://github.com/astral-sh/ty/issues/1465
synced_at: 2026-01-10T02:06:25Z
```

# ty incorrectly infers tensor arithmetic expression as `int | float` instead of `Tensor`

---

_Issue opened by @anmorgunov on 2025-11-02 15:02_

### Summary

When performing arithmetic operations on PyTorch tensors (e.g., `1.0 / (base ** (torch.arange(...).float() / dim))`), ty incorrectly infers the result type as `int | float` instead of `Tensor`. This causes false positive errors
when passing the result to methods like `nn.Module.register_buffer()`, which expect a `Tensor | None`. mypy correctly infers the type.

```py
import torch
import torch.nn as nn


class SimpleModule(nn.Module):
    """A simple module that demonstrates the register_buffer type inference issue."""

    def __init__(self, dim: int):
        super().__init__()

        # This works fine - explicit Tensor
        tensor_explicit = torch.randn(dim)
        self.register_buffer("explicit_buffer", tensor_explicit)

        # This is the problem - ty complains about this
        # The expression (1.0 / base**(...)) returns a Tensor, but ty infers int | float
        base = 10000
        inv_freq = 1.0 / (base ** (torch.arange(0, dim, 2).float() / dim))
        self.register_buffer("inv_freq", inv_freq, persistent=False)

```

uvx ty check returns:

```
uvx ty check register_buffer_repro.py
error[invalid-argument-type]: Argument to bound method `register_buffer` is incorrect
  --> register_buffer_repro.py:20:42
   |
18 |         base = 10000
19 |         inv_freq = 1.0 / (base ** (torch.arange(0, dim, 2).float() / dim))
20 |         self.register_buffer("inv_freq", inv_freq, persistent=False)
   |                                          ^^^^^^^^ Expected `Tensor | None`, found `int | float`
21 |         # ty error: Expected `Tensor | None`, found `int | float`
   |
info: Method defined here
   --> .venv/lib/python3.13/site-packages/torch/nn/modules/module.py:525:9
    |
523 |     forward: Callable[..., Any] = _forward_unimplemented
524 |
525 |     def register_buffer(
    |         ^^^^^^^^^^^^^^^
526 |         self, name: str, tensor: Optional[Tensor], persistent: bool = True
    |                          ------------------------ Parameter declared here
527 |     ) -> None:
528 |         r"""Add a buffer to the module.
    |
info: rule `invalid-argument-type` is enabled by default
```


### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Label `bug` added by @sharkdp on 2025-11-03 18:44_

---

_Label `type-inference` added by @sharkdp on 2025-11-03 18:44_

---

_Comment by @sharkdp on 2025-11-03 18:46_

Thank you for reporting this.

I can't fully confirm this at the moment as my connection is too bad to download pytorch, but it looks like we're not respecting [`__rpow__` on `Tensor`](https://github.com/pytorch/pytorch/blob/f3913ea641d871f04fa2b6588a77f63efeeb9f10/torch/_tensor.py#L1112-L1113) correctly.

An MRE would be:
```py
class Tensor:
    def __rpow__(self, other) -> Tensor:
        return self

reveal_type(2 ** Tensor())  # int, should be Tensor
```
https://play.ty.dev/ba18a5e4-18ff-4903-9000-6485079a6eb4

---

_Comment by @anmorgunov on 2025-11-03 18:56_

can confirm that it comes from raising to the power

```py
import torch

inv_freq = 2**torch.Tensor([1, 2, 3, 4, 5])

reveal_type(inv_freq)
```


```py
info[revealed-type]: Revealed type
 --> check.py:5:13
  |
3 | inv_freq = 2**torch.Tensor([1, 2, 3, 4, 5])
4 |
5 | reveal_type(inv_freq)
  |             ^^^^^^^^ `int`
```



---

_Comment by @carljm on 2025-11-03 20:26_

Thanks for the report! The root cause of this (thanks @AlexWaygood) is that the signature of `int.__pow__` is defined using a PEP 613 type alias in typeshed (`_PositiveInteger`), which we don't yet support correctly, so we wrongly think that the call to `int.__pow__` succeeds:

https://github.com/astral-sh/ruff/blob/fe4ee81b9749bd326845aec30e694af8568549e6/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L553-L568

Once we support PEP 613, we will find that none of the `int.__pow__` overloads match, which will then cause us to fallback to using `Tensor.__rpow__` to infer the result of this expression.

---

_Closed by @carljm on 2025-11-03 20:26_

---

_Comment by @carljm on 2025-11-12 01:27_

Confirmed this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
