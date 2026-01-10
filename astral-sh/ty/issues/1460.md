```yaml
number: 1460
title: "treat `__setattr__` as fallback only"
type: issue
state: closed
author: lbhm
labels:
  - runtime semantics
assignees: []
created_at: 2025-10-31T09:49:23Z
updated_at: 2025-12-31T00:01:12Z
url: https://github.com/astral-sh/ty/issues/1460
synced_at: 2026-01-10T01:56:40Z
```

# treat `__setattr__` as fallback only

---

_Issue opened by @lbhm on 2025-10-31 09:49_

### Question

I recently noticed that ty complains about attribute assignment in `torch.nn.Module` subclasses that other type checkers do not complain about. If I run ty on the following MWE:

```python
import torch

class MyModule(torch.nn.Module):
    def __init__(self, param: int = 42) -> None:
        self.some_param = param
```

ty complains with the error:

```
error[unresolved-attribute]: Can not assign object of type `int` to attribute `some_param` on type `Self@__init__` with custom `__setattr__` method.
 --> ty_test.py:6:9
  |
4 | class MyModule(torch.nn.Module):
5 |     def __init__(self, param: int = 42) -> None:
6 |         self.some_param = param
  |         ^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

However, if I instead use `self.some_param: int = param`, ty does not complain. Is this intended behavior? Other type checkers do not complain, since the type seems clear from the method signature.

The root cause seems to be `torch.nn.Module`'s custom `__setattr__` implementation, which claims that it only accepts `Union[Tensor, "Module"]`, but in reality forwards any other types to `super().__setattr__` (see [here](https://github.com/pytorch/pytorch/blob/main/torch/nn/modules/module.py#L2078-L2079)
).

### Version

ty 0.0.1-alpha.25

---

_Label `question` added by @lbhm on 2025-10-31 09:49_

---

_Renamed from "Assigning non-`Parameter` attributes in `torch.nn.Module` subclass" to "Assigning non-{`Tensor`/`Module`} attributes in `torch.nn.Module` subclasses" by @lbhm on 2025-10-31 09:50_

---

_Comment by @sharkdp on 2025-10-31 11:09_

Thank you for reporting this.

> The root cause seems to be `torch.nn.Module`'s custom `__setattr__` implementation, which claims that it only accepts `Union[Tensor, "Module"]`, but in reality forwards any other types to `super().__setattr__` (see [here](https://github.com/pytorch/pytorch/blob/main/torch/nn/modules/module.py#L2078-L2079)
> ).

That seems problematic. `__setattr__` takes precedence, and its signature clearly states that it won't accept values of type `int`. I wonder under which assumption other type checkers exempt this `__setattr__` call from proper call validation.

> However, if I instead use `self.some_param: int = param`, ty does not complain. Is this intended behavior? Other type checkers do not complain, since the type seems clear from the method signature.

That is an interesting observation. I'm pretty sure this was not an intentional decision, even if it looks like a reasonable thing to do(?).


FWIW, an MRE:
```py
class C:
    def __setattr__(self, name: str, value: bytes):
        ...

class M(C):
    def __init__(self):
        self.value = 1  # ty: Can not assign object of type `Literal[1]` to attribute `value` on type `Self@__init__` with custom `__setattr__` method
```

---

_Comment by @carljm on 2025-10-31 20:06_

It seems like other type-checkers only consider `__setattr__` as a fallback, when there is otherwise no attribute of that name. This seems strange, since at runtime `__setattr__` always takes precedence. I guess the rationale here is that if you define a `__setattr__` and also explicitly create some named attributes, it's on you to ensure that at runtime your `__setattr__` actually handles those named attributes correctly, consistently with how you've annotated them -- and you aren't necessarily expected to repeat the types of those attributes in your annotations of `__setattr__`.

Not sure if I agree this is the best choice, but I think we need to modify our behavior accordingly or we'll be too incompatible.

---

_Label `question` removed by @carljm on 2025-10-31 20:06_

---

_Label `runtime semantics` added by @carljm on 2025-10-31 20:06_

---

_Added to milestone `GA` by @carljm on 2025-10-31 20:06_

---

_Renamed from "Assigning non-{`Tensor`/`Module`} attributes in `torch.nn.Module` subclasses" to "treat `__setattr__` as fallback only" by @carljm on 2025-10-31 20:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 00:28_

---

_Closed by @charliermarsh on 2025-12-31 00:01_

---
