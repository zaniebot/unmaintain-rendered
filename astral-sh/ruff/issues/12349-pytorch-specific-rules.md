---
number: 12349
title: PyTorch-specific rules
type: issue
state: open
author: sbrugman
labels:
  - rule
assignees: []
created_at: 2024-07-16T16:42:54Z
updated_at: 2025-06-16T17:18:48Z
url: https://github.com/astral-sh/ruff/issues/12349
synced_at: 2026-01-10T01:22:52Z
---

# PyTorch-specific rules

---

_Issue opened by @sbrugman on 2024-07-16 16:42_

Introducing lints for pytorch expands `ruff` utility in the AI/machine learning space, in addition to [Numpy](https://github.com/astral-sh/ruff/issues/2455) and [Spark](https://github.com/astral-sh/ruff/issues/7272). Detecting potential errors during linting has the potential to save costly GPU compute.



---

_Label `rule` added by @MichaReiser on 2024-07-16 16:47_

---

_Comment by @randolf-scholz on 2024-07-18 11:56_

Also #9664.

---

_Comment by @njzjz on 2024-07-21 23:28_

There is a flake8 plugin called TorchFix developed by the PyTorch team: https://github.com/pytorch-labs/torchfix

---

_Comment by @sbrugman on 2024-07-22 06:56_

> There is a flake8 plugin called TorchFix developed by the PyTorch team: https://github.com/pytorch-labs/torchfix

Thanks for sharing! This would be a great base to start from!

---

_Comment by @NeilGirdhar on 2024-07-23 02:59_

> TOR008 -> Use array agnostic syntax (see also https://github.com/astral-sh/ruff/issues/8615), e.g. use x.mean() rather than torch.mean(x)

I think it would be better to move towards the Array API, which is the most "array agnostic".  This eliminates any peculiarities of PyTorch versus NumPy as well.  So:
```python
xp = get_namespace(x)
xp.mean(x)
```
This works with NumPy, PyTorch, Jax, TensorFlow, CuPy, etc.

---

_Comment by @neosr-project on 2024-10-17 20:18_

+1 for TorchFix. It integrates with flake8, so it should be easy to port the rules. It has been officially released now with the update to pytorch 2.5.

---

_Comment by @randolf-scholz on 2025-01-06 16:29_

Rule: detect usage of `x * x.log()` (entropy), advise users to use special function `torch.special.xlogy` instead. (This bug even occurs in torch source code!)

---

_Comment by @isaaccorley on 2025-06-16 17:18_

bump -- would also love to use this with ruff!

---

_Referenced in [torchgeo/torchgeo#2839](../../torchgeo/torchgeo/issues/2839.md) on 2025-06-18 12:33_

---
