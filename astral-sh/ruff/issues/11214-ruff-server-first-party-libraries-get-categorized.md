---
number: 11214
title: "`ruff server` first-party libraries get categorized as third-party"
type: issue
state: closed
author: bluthej
labels:
  - bug
  - server
assignees: []
created_at: 2024-04-30T14:25:23Z
updated_at: 2024-05-02T02:24:37Z
url: https://github.com/astral-sh/ruff/issues/11214
synced_at: 2026-01-10T01:22:50Z
---

# `ruff server` first-party libraries get categorized as third-party

---

_Issue opened by @bluthej on 2024-04-30 14:25_

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
Keywords: I001, Organize imports, first-party

Ruff version : 0.4.2

If I'm not mistaken, `ruff server` currently treats first-party libraries like they're third-party libraries, which is not the case for `ruff-lsp`.

Here is a minimal setup to reproduce this:
- Create a new project with the following layout:
```shell
.
├── pyproject.toml
└── src
     └── hello_world
         ├── __init__.py
         ├── module.py
         └── other_module.py
```
with these contents:
<details><summary>pyproject.toml</summary>
<p>

```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "hello-world"
version = "0.0.1"
dependencies = [
  "numpy"
]

[tool.ruff]
lint.extend-select = ["I"]
```

</p>
</details> 

<details><summary>module.py</summary>
<p>

```python
hello = "hello world"
```

</p>
</details> 

<details><summary>other_module.py</summary>
<p>

```python
from pathlib import Path

from numpy.linalg import norm

from hello_world.module import hello
```

</p>
</details> 

- Then run `ruff-lsp` in an editor => this should not lint the import order, just like doing `ruff check .` doesn't say anything about import order
- Then run `ruff server` in an editor => now there should be an `Organize import` lint, and if you apply it, it should bring the `hello_world` import just above the `numpy` import, which is not the expected behavior

---

_Label `server` added by @snowsignal on 2024-04-30 14:39_

---

_Label `bug` added by @snowsignal on 2024-04-30 14:39_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-30 14:40_

---

_Comment by @charliermarsh on 2024-04-30 15:57_

It's similar to #11185: we need to determine and provide the containing package at that call site.

---

_Referenced in [astral-sh/ruff#11224](../../astral-sh/ruff/pulls/11224.md) on 2024-05-01 00:26_

---

_Closed by @snowsignal on 2024-05-02 02:24_

---

_Closed by @snowsignal on 2024-05-02 02:24_

---
