```yaml
number: 3769
title: False positive in D402
type: issue
state: open
author: DeD1rk
labels:
  - docstring
assignees: []
created_at: 2023-03-28T13:30:28Z
updated_at: 2023-03-28T15:00:43Z
url: https://github.com/astral-sh/ruff/issues/3769
synced_at: 2026-01-12T15:54:44Z
```

# False positive in D402

---

_@DeD1rk_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I stumbled upon a false positive for pydocstyle's D402 (First line should not be the function's signature) in ruff 0.0.259, python 3.8.10.

I'm not sure what pydocstyle itself would do, but it seems incorrect even if pydocstyle would also be triggered:

```python
class A(models.QuerySet):
    def b(self):
        """Call `C.b()` on something."""
        pass
```

Here the `b(`in the docstring seems to cause the violation. The rule is also triggered by `"""Call 'C.b(x)' on something."""`, `"""Call 'b(' on something."""` and `"""Call 'b(a,c)' on something."""`, but not if it does not contain `b(`.

---

_Comment by @charliermarsh on 2023-03-28 15:00_

Yeah FWIW pydocstyle also flags this. I can't think of a more reliable way to implement this off the top of my head while still catching a reasonable number of true positives.

---

_Label `docstring` added by @charliermarsh on 2023-03-28 15:00_

---
