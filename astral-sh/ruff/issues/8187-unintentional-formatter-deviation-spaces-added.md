```yaml
number: 8187
title: "Unintentional formatter deviation: spaces added around power operator (`**`)"
type: issue
state: closed
author: stinodego
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-24T22:04:46Z
updated_at: 2023-10-25T06:24:08Z
url: https://github.com/astral-sh/ruff/issues/8187
synced_at: 2026-01-10T11:09:50Z
```

# Unintentional formatter deviation: spaces added around power operator (`**`)

---

_Issue opened by @stinodego on 2023-10-24 22:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The power operator is usually used without surrounding spaces to indicate precendence over other operators. The following code is left alone by both black and ruff:

```python
x = 2**2
```

However, when the right operand is `None`, ruff adds spaces, while black leaves it. In this case, I think black is correct.

```diff
- x = 2**None
+ x = 2 ** None
```

This change is undesirable. Note that this code does not run (TypeError) - but we have something similar in our test suite where we test the `__pow__`/`__rpow__` implementation on some of our types.

Command: `ruff format .`
Formatter settings: None
Version: ruff `0.1.2`


---

_Label `formatter` added by @MichaReiser on 2023-10-24 23:23_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-24 23:23_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-24 23:28_

---

_Label `bug` added by @MichaReiser on 2023-10-24 23:28_

---

_Closed by @MichaReiser on 2023-10-25 06:24_

---
