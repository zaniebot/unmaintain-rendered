---
number: 11656
title: PLR6104 misses cases with chained associative operators
type: issue
state: open
author: Avasam
labels: []
assignees: []
created_at: 2024-05-31T21:51:15Z
updated_at: 2024-06-01T22:33:33Z
url: https://github.com/astral-sh/ruff/issues/11656
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR6104 misses cases with chained associative operators

---

_Issue opened by @Avasam on 2024-05-31 21:51_

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

The following case:
```py
test = 1
test3 = 3
test = test * 2 * test3 * 4
96
```

is probably safe as:
```py
test = 1
test3 = 3
test *= 2 * test3 * 4
96
```

or am I missing something?

Similarly for concatenation (which is non-commutative, but associative), seems fine here:
```py
test = "1"
test3 = "3"
test = test + "2" + test3 + "4"
'1234'
```

```py
test = "1"
test3 = "3"
test += "2" + test3 + "4"
'1234'
```

Example of an operator where this doesn't work (division):
```py
test = 1
test3 = 3
test = test / 2 / test3 / 4
0.041666666666666664
```

```py
test = 1
test3 = 3
test /= 2 / test3 / 4
6.0
```

(also just for fun, `/` is technically a concatenation operator for `Path`, but that'd require checking for the type)


ruff version 0.4.7

---

_Renamed from "PLR6104 misses cases with multiple identical commutative operators" to "PLR6104 misses cases with chained identical commutative operators" by @Avasam on 2024-05-31 21:53_

---

_Renamed from "PLR6104 misses cases with chained identical commutative operators" to "PLR6104 misses cases with chained associative operators" by @Avasam on 2024-06-01 00:20_

---

_Referenced in [mhammond/pywin32#2274](../../mhammond/pywin32/pulls/2274.md) on 2024-06-01 03:02_

---
