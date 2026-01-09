---
number: 11166
title: "UP031: ruff 0.4.2 generated invalid fix it code"
type: issue
state: open
author: Skylion007
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-04-26T17:44:17Z
updated_at: 2024-05-03T16:16:50Z
url: https://github.com/astral-sh/ruff/issues/11166
synced_at: 2026-01-07T13:12:15-06:00
---

# UP031: ruff 0.4.2 generated invalid fix it code

---

_Issue opened by @Skylion007 on 2024-04-26 17:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```python
                msg="\n".join(
                    [
                        "alpha = alpha_c + %.2g" % shift,
                        "expected_grad: %.5g" % expected_grad,
                        "actual_grad: %.5g" % actual_grad,
                        "error = %.2g" % torch.abs(expected_grad - actual_grad).max(),
                    ]
```
became
```python
                msg="\n".join(
                    [
                        f"alpha = alpha_c + {shift:.2g}",
                        f"expected_grad: {expected_grad:.5g}",
                        f"actual_grad: {actual_grad:.5g}",
                        f"error = {torch.abs(expected_grad - actual_grad).max():.2g}",
                    ]
 ```
which throws an error. I know it's an unsafe fix, but unsafe fixes should not generate invalid code. Specifically:
```
Traceback (most recent call last):
  File "/var/lib/jenkins/workspace/test/distributions/test_distributions.py", line 4658, in test_dirichlet_multivariate
    f"expected_grad: {expected_grad:.5g}",
  File "/opt/conda/envs/py_3.10/lib/python3.10/site-packages/torch/_tensor.py", line 989, in __format__
    return object.__format__(self, format_spec)
TypeError: unsupported format string passed to Tensor.__format__
```
from this PR: https://github.com/pytorch/pytorch/pull/125031/

---

_Label `bug` added by @AlexWaygood on 2024-04-26 17:52_

---

_Label `fixes` added by @AlexWaygood on 2024-04-26 17:52_

---

_Comment by @AlexWaygood on 2024-04-26 17:54_

Guessing this is due to [the recent change to UP031](https://github.com/astral-sh/ruff/pull/11019)? Cc. @plredmond

---

_Comment by @charliermarsh on 2024-04-26 18:01_

Why can't tensors be passed-in there, but can be formatted with `%` formatting?

---

_Comment by @Skylion007 on 2024-04-26 18:09_

@charliermarsh I think it's related to classes overriding `object.__format__` magic method. Does the '%' method cast it to a string before calling `__format__` on it or something?

---

_Comment by @Skylion007 on 2024-04-26 18:15_

@charliermarsh Ah, apparently in the first code, it's implicitly converted before: "Here, you're using the old-style string formatting (%-formatting), where shift, a PyTorch tensor, is implicitly converted to a numeric type (float or integer) when it's formatted. This conversion happens before the formatting is applied, so the process doesn't raise any issues."

```python
import torch
x = torch.tensor([1.0])
print("Value: %.5g" % x)  # Works because x is cast as a float before __format__ is called
print(f"Value: {x:.5g}") # fails because Tensor.__format__ is called
```

---

_Renamed from "Ruff: 0.4.2 generated invalid fix it code" to "UP031: ruff 0.4.2 generated invalid fix it code" by @Skylion007 on 2024-04-26 18:17_

---

_Comment by @Skylion007 on 2024-04-28 17:00_

I think explicitly casting to float in all float specific f-string fixits could solve this issue?

---

_Comment by @charliermarsh on 2024-04-28 18:18_

I think that makes sense. E.g., when there's a `:.5g` specifier?

---

_Comment by @Skylion007 on 2024-04-28 18:26_

There are probably other specifies that do this casting f (or maybe some to int for d) etc.

---

_Comment by @plredmond on 2024-05-03 16:16_

@charliermarsh perhaps we could add this kind of explicit-conversion to the UP031 rule after we can infer the types of the expressions? alternatively, since we parse the format string we could use that information to select an explicit-conversion function?

---
