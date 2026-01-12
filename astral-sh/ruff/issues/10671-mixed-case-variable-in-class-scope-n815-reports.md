```yaml
number: 10671
title: mixed-case-variable-in-class-scope (N815) reports mixedCase on class variables of subclassed TypedDict
type: issue
state: closed
author: teaishealthy
labels:
  - type-inference
assignees: []
created_at: 2024-03-30T16:47:47Z
updated_at: 2024-10-24T14:34:14Z
url: https://github.com/astral-sh/ruff/issues/10671
synced_at: 2026-01-12T15:54:50Z
```

# mixed-case-variable-in-class-scope (N815) reports mixedCase on class variables of subclassed TypedDict

---

_@teaishealthy_


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

## Description
`ruff 0.1.15`

N815 will report on mixedCase class variables on subclasses of a TypedDict, while it won't on the actual TypedDict (as expected)

## Minimal reproducible example 

```py
from typing import TypedDict

class A(TypedDict):
    mixedCase: int  # no error!

class B(A):
    alsoMixedCase: int  # Variable `alsoMixedCase` in class scope should not be mixedCase

 ```



	


---

_Comment by @charliermarsh on 2024-04-01 01:23_

We can only support this for subclasses defined in the same file right now.

---

_Label `multifile-analysis` added by @charliermarsh on 2024-04-01 01:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-01 17:23_

---

_Closed by @charliermarsh on 2024-04-01 17:40_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
