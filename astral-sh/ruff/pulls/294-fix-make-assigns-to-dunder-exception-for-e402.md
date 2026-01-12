```yaml
number: 294
title: "fix: Make assigns to dunder exception for E402."
type: pull_request
state: merged
author: sgryjp
labels: []
assignees: []
merged: true
base: main
head: fix/e402-dunders
created_at: 2022-10-01T10:41:48Z
updated_at: 2022-10-01T13:45:07Z
url: https://github.com/astral-sh/ruff/pull/294
synced_at: 2026-01-12T05:48:45Z
```

# fix: Make assigns to dunder exception for E402.

---

_Pull request opened by @sgryjp on 2022-10-01 10:41_

This commit allow assignments to dunder names to be placed after a before normal imports. PEP 8 says:

> Module level "dunders" (i.e. names with two leading and two trailing
> underscores) such as __all__, __author__, __version__, etc. should be
> placed after the module docstring but before any import statements
> except from __future__ imports.

Note that the checking logic is a copy of [that of pycodestyle](https://github.com/PyCQA/pycodestyle/blob/2.9.1/pycodestyle.py#L1153).

---

_Comment by @charliermarsh on 2022-10-01 13:43_

This is a great PR. Thank you for putting it together!

---

_Merged by @charliermarsh on 2022-10-01 13:43_

---

_Closed by @charliermarsh on 2022-10-01 13:43_

---

_Branch deleted on 2022-10-01 13:45_

---
