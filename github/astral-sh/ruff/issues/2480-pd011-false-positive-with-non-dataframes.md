---
number: 2480
title: "`PD011`: False positive with non-dataframes"
type: issue
state: closed
author: ngnpope
labels: []
assignees: []
created_at: 2023-02-02T14:43:59Z
updated_at: 2023-02-03T18:15:01Z
url: https://github.com/astral-sh/ruff/issues/2480
synced_at: 2026-01-07T13:12:14-06:00
---

# `PD011`: False positive with non-dataframes

---

_Issue opened by @ngnpope on 2023-02-02 14:43_

```python
class A:
    values: int

A().values
```
Results in:
```console
❯ ruff --version
ruff 0.0.239

❯ ruff --select PD011 bug.py 
bug.py:4:1: PD011 Use `.to_numpy()` instead of `.values`
Found 1 error.
```

---

_Comment by @charliermarsh on 2023-02-02 20:54_

Similar to #2229. It's just really hard to avoid these without making the rules mostly useless. The best we could do is gate on the presence of `import pandas`, but even then you could have false positives, and you'd get more false negatives, since you don't have to import Pandas in a given file in order to work with DataFrames.

---

_Comment by @JonathanPlasse on 2023-02-02 21:21_

Tell me if I am wrong, but could it be resolved if we tracked the types of variables?
Is it planned to support it?
For this would need first to have multiple files support.

---

_Comment by @charliermarsh on 2023-02-02 21:23_

It could definitely be resolved if we had full type inference!

---

_Comment by @ngnpope on 2023-02-02 22:50_

Yeah, it's a hard one without typing. I'll ignore for now. Sorry for the duplicate

---

_Comment by @charliermarsh on 2023-02-03 18:15_

All good!

---

_Closed by @charliermarsh on 2023-02-03 18:15_

---

_Referenced in [maddenp-cu/wxvx#14](../../maddenp-cu/wxvx/pulls/14.md) on 2025-06-04 17:30_

---
