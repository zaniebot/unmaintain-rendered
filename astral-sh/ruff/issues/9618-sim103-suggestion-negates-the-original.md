```yaml
number: 9618
title: SIM103 suggestion negates the original conditional expression
type: issue
state: closed
author: chunyang
labels:
  - bug
assignees: []
created_at: 2024-01-23T01:38:27Z
updated_at: 2024-01-23T05:08:08Z
url: https://github.com/astral-sh/ruff/issues/9618
synced_at: 2026-01-10T11:09:51Z
```

# SIM103 suggestion negates the original conditional expression

---

_Issue opened by @chunyang on 2024-01-23 01:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Consider the following Python code:
```python
def opposite(a: bool) -> bool:
    if a:
        return False
    else:
        return True
```

When I run ruff on the code with the SIM checks selected, the suggested replacement returns the opposite of the expected value.

```
% ruff check --isolated --select SIM --show-source sim103.py 
sim103.py:2:5: SIM103 Return the condition `a` directly
  |
1 |   def opposite(a: bool) -> bool:
2 |       if a:
  |  _____^
3 | |         return False
4 | |     else:
5 | |         return True
  | |___________________^ SIM103
  |
  = help: Replace with `return a`
```

If I switch `False` and `True` in the code, the suggested replacement remains the same, but is then correct.

```bash
% ruff --version
ruff 0.1.14
```

Playground link: https://play.ruff.rs/7d91158b-0d65-4cc1-95f3-435136f0d1e9

---

_Label `bug` added by @charliermarsh on 2024-01-23 01:43_

---

_Comment by @chunyang on 2024-01-23 01:46_

Seems related to #2037 but IIUC that only disabled the autofix, but didn't fix the suggestion.

---

_Comment by @charliermarsh on 2024-01-23 02:23_

Ah thanks -- that does look like a bug.

---

_Comment by @charliermarsh on 2024-01-23 02:24_

I'll re-enable the fix, and insert the correct code and suggestion.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-23 02:54_

---

_Closed by @charliermarsh on 2024-01-23 03:26_

---

_Comment by @charliermarsh on 2024-01-23 03:34_

Fixed in the next release, thanks!

---

_Comment by @chunyang on 2024-01-23 05:08_

Thanks for the quick fix!

---
