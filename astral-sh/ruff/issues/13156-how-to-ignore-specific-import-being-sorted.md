```yaml
number: 13156
title: How to ignore specific import being sorted
type: issue
state: closed
author: myslak71
labels:
  - question
  - isort
  - configuration
assignees: []
created_at: 2024-08-30T07:48:33Z
updated_at: 2024-09-02T07:52:55Z
url: https://github.com/astral-sh/ruff/issues/13156
synced_at: 2026-01-12T15:54:52Z
```

# How to ignore specific import being sorted

---

_@myslak71_

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

I wanna ignore specific import line from being sorted since I wanna have it imported first but I'd like to organize all remaining imports.
```
from tracer import tracer  # noqa: I001

from bmodule import importb
from amodule import a
```

`# fmt: skip` is being ignored completely - so it organizes imports anyway
` # noqa: I001` stops ordering completely even for lines without `noqa`

Im running ruff check --select I

---

_Label `isort` added by @AlexWaygood on 2024-08-30 10:39_

---

_Comment by @AlexWaygood on 2024-08-30 10:42_

Hi! I think our [`lint.isort.force-to-top` setting](https://docs.astral.sh/ruff/settings/#lint_isort_force-to-top) will do this for you. E.g. in a `pyproject.toml` file:

```toml
[tool.ruff.lint.isort]
force-to-top = ["tracer"]
```

Does that work?

---

_Label `question` added by @AlexWaygood on 2024-08-30 10:42_

---

_Label `configuration` added by @AlexWaygood on 2024-08-30 10:42_

---

_Comment by @myslak71 on 2024-08-30 11:08_

it works partially - it just moves the import to the top of own modules - is there a way to move it all the way to the top even before builtin modules?

---

_Comment by @AlexWaygood on 2024-08-30 11:19_

Yeah, you can define a custom section and put `tracer` in that section, then use `section-order` to enforce that that section comes before all others:

```toml
[tool.ruff.lint.isort]
sections = {"tracer" = ["tracer"]}
section-order = [
    "future",
    "tracer",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]
```

- https://docs.astral.sh/ruff/settings/#lint_isort_sections
- https://docs.astral.sh/ruff/settings/#lint_isort_section-order

---

_Closed by @MichaReiser on 2024-09-02 07:52_

---
