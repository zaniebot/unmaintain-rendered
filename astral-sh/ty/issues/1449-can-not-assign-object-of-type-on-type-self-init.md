```yaml
number: 1449
title: "Can not assign object of type ... on type `Self@__init__` with custom `__setattr__` method"
type: issue
state: closed
author: karlicoss
labels:
  - diagnostics
assignees: []
created_at: 2025-10-28T00:35:17Z
updated_at: 2025-11-03T18:27:45Z
url: https://github.com/astral-sh/ty/issues/1449
synced_at: 2026-01-10T02:06:25Z
```

# Can not assign object of type ... on type `Self@__init__` with custom `__setattr__` method

---

_Issue opened by @karlicoss on 2025-10-28 00:35_

### Summary

While trying to type check some torch code on `0.0.1a24`, I got some confusing errors, which I narrowed down to this minimal example

https://play.ty.dev/cda201e7-dab6-422d-9e90-c03c2268a000

```
Can not assign object of type `Literal[123]` to attribute `attr` on type `Self@__init__` with custom `__setattr__` method. (unresolved-attribute) [Ln 11, Col 9]
```

If I change type of `value: ` to `int` or `Any`, this type checks in `ty`. I guess `ty` is correct, it's just the error message could be improved?

Also interesting enough, both ty and mypy reveal the type of `self.attr` as `Literal[123]` or `int`, however in runtime this would fail because I haven't actually implemented the `__setattr__` properly. I guess this is just best effort heuristics since we don't know what happens inside `__setattr__`?




### Version

0.0.1a24 (latest/playground version db0e921db)

---

_Comment by @karlicoss on 2025-10-28 00:43_

Also in case someone else runs into this -- the original torch code that `ty` complained about was something like this:

```
import torch.nn as nn

class Model(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.count = 123

    def forward(self):
        return self.count


model = Model()
print(model.forward())
```

The error was coming from `value: Union[Tensor, "Module"]` defined [here](https://github.com/pytorch/pytorch/blob/47f50cfd456313d8b46fcc7a1f6de477aa0a5aee/torch/nn/modules/module.py#L1976-L1983). It worked in runtime for my somewhat unorthodox usecase of torch, but guess `ty` is correct and I should use `nn.Parameter` or `Buffer` instead.

---

_Comment by @sharkdp on 2025-10-28 09:18_

> If I change type of `value: ` to `int` or `Any`, this type checks in `ty`. I guess `ty` is correct, it's just the error message could be improved?

Yes, I think ty is behaving as expected here, but we can certainly try to improve the error message. I assume it would have been more helpful if we would clearly state something like: "The assignment to the attribute `attr` calls the custom `__setattr__` method defined at …, but the call failed for the following reason: Expected `str` for parameter `value`, got argument of type `int`"?

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-28 09:21_

---

_Comment by @weebao on 2025-11-01 23:57_

I'm also getting this for
```python
class Model(nn.Module):
    def __init__(self, a=4):
        self.a = a
```
```
Can not assign object of type `Unknown | Literal[4]` 
to attribute `a` on type `Self@__init__` with custom `__setattr__` method. (ty-unresolved-attribute)
```

I believe ty's mantra is that removing a type annotation should not cause a type error.

---

_Comment by @carljm on 2025-11-03 18:27_

This is the same issue as https://github.com/astral-sh/ty/issues/1460 -- closing this one as duplicate since that one already has a proposed direction (we should handle `__setattr__` precedence more like how other type checkers handle it.)

---

_Closed by @carljm on 2025-11-03 18:27_

---
