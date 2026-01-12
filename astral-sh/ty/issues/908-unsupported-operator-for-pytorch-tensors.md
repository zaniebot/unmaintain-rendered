```yaml
number: 908
title: unsupported-operator for pytorch tensors
type: issue
state: closed
author: PetterS
labels: []
assignees: []
created_at: 2025-07-28T08:28:34Z
updated_at: 2025-10-14T12:27:54Z
url: https://github.com/astral-sh/ty/issues/908
synced_at: 2026-01-12T15:54:24Z
```

# unsupported-operator for pytorch tensors

---

_@PetterS_

### Summary

Example:
```python
import torch
t = torch.zeros((10, 10))
t ** 2
```
ty:
```
error[unsupported-operator]: Operator `**` is unsupported between objects of type `Tensor` and `Literal[2]`
 --> ty_test.py:6:1
  |
5 | t = torch.zeros((10, 10))
6 | t ** 2
  | ^^^^^^
  |
info: rule `unsupported-operator` is enabled by default

Found 1 diagnostic
```

Other operators like `@` and `+` work.

### Version

ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)

---

_Comment by @sharkdp on 2025-07-28 09:38_

Thank you for reporting this.

The definition of `Tensor.__pow__` is here:

https://github.com/pytorch/pytorch/blob/f3913ea641d871f04fa2b6588a77f63efeeb9f10/torch/_tensor.py#L1084-L1092

At the moment, ty does *not* assume that a `Callable` attribute on a class is necessarily a descriptor. So when accessing `t.__pow__`, we assume that it does not bind the `self` argument. Consequently, calling `t.__pow__(2)` (which is more or less what happens when doing `t**2`), this leads to a `invalid-argument-type` error:

```
error[invalid-argument-type]: Argument is incorrect
 --> main.py:6:11
  |
5 | t = torch.zeros((10, 10))
6 | t.__pow__(2)
  |           ^ Expected `TensorBase`, found `Literal[2]`
```

This `invalid-argument-type` error is what causes the `unsupported-operator` on `t**2` (we should improve our diagnostics to make the root cause clear).

The behavior described above is a [known problem](https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938) in Python's type system. We are aware about the issue, and progress is currently tracked in https://github.com/astral-sh/ty/issues/491. So I'll close this as a duplicate, but mention your case in that ticket.

---

_Closed by @sharkdp on 2025-07-28 09:38_

---

_Comment by @PetterS on 2025-07-28 12:31_

Thanks for the explanation!

---

_Closed by @sharkdp on 2025-10-14 12:27_

---
