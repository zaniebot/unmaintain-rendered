```yaml
number: 10928
title: "Add `honor-case-in-force-sorted-sections` flag for `isort`"
type: issue
state: closed
author: serjflint
labels:
  - question
  - isort
assignees: []
created_at: 2024-04-14T16:06:14Z
updated_at: 2024-04-24T12:06:30Z
url: https://github.com/astral-sh/ruff/issues/10928
synced_at: 2026-01-12T15:54:50Z
```

# Add `honor-case-in-force-sorted-sections` flag for `isort`

---

_@serjflint_

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

Hi!

ruff 0.3.4

I am trying to sort imports like `ruff check package/module.py --fix-only`

```python
from typing import cast
from typing import Union
```

my config

```toml
[tool.ruff.lint]
select = ["I"]

[tool.ruff.lint.isort]
case-sensitive = true
force-single-line = true
force-sort-within-sections = true
order-by-type = false
```

I get 

```python
from typing import Union
from typing import cast
```

Instead of the file staying the same.

Please add the option from isort
https://pycqa.github.io/isort/docs/configuration/options.html#honor-case-in-force-sorted-sections

---

_Label `isort` added by @AlexWaygood on 2024-04-14 16:08_

---

_Label `question` added by @MichaReiser on 2024-04-15 06:55_

---

_Comment by @samdoran on 2024-04-22 20:35_

I was struggling with this same behavior. Adding `order-by-type = false` with `case-sensitive = false` as suggested [in this comment](https://github.com/astral-sh/ruff/issues/4670#issuecomment-1920954543) is what fixed it for me.

---

_Comment by @dhruvmanila on 2024-04-24 06:49_

@serjflint Does the solution suggested by @samdoran solve your use-case? It does at least for the provided code snippet: https://play.ruff.rs/e90de266-3b0d-44cb-8955-f0c9bfcad5de

---

_Comment by @serjflint on 2024-04-24 12:06_





> I was struggling with this same behavior. Adding `order-by-type = false` with `case-sensitive = false` as suggested [in this comment](https://github.com/astral-sh/ruff/issues/4670#issuecomment-1920954543) is what fixed it for me.



> @serjflint Does the solution suggested by @samdoran solve your use-case? It does at least for the provided code snippet: https://play.ruff.rs/e90de266-3b0d-44cb-8955-f0c9bfcad5de

Hi! Yes, it worked for me. Thank you.

---

_Closed by @serjflint on 2024-04-24 12:06_

---
