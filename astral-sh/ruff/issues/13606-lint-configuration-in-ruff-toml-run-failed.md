```yaml
number: 13606
title: "`[lint.[*]]` configuration in .ruff.toml run failed."
type: issue
state: closed
author: YaoJusheng
labels:
  - question
assignees: []
created_at: 2024-10-02T13:31:37Z
updated_at: 2024-10-03T06:41:42Z
url: https://github.com/astral-sh/ruff/issues/13606
synced_at: 2026-01-12T15:54:53Z
```

# `[lint.[*]]` configuration in .ruff.toml run failed.

---

_@YaoJusheng_

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

execute:  `ruff check . ` return error outputs:
```                                                                                                                                                                                                                                   
ruff failed
  Cause: Failed to parse xxx/.ruff.toml
  Cause: TOML parse error at line 87, column 1
   |
87 | [lint]
   | ^^^^^^
unknown field `select`, expected one of `convention`, `ignore-decorators`, `property-decorators`
```

the content of .ruff.toml:
```
...
[lint]
# Enable preview features.
preview = true
# On top of the default `select` (`E4`, E7`, `E9`, and `F`), enable flake8-bugbear (`B`) and flake8-quotes (`Q`).
extend-select = ["B", "Q"]


[lint.flake8-type-checking]
exempt-modules = ["typing", "typing_extensions"]

[lint.isort]
force-wrap-aliases = true
combine-as-imports = true
from-first = false
length-sort = true
no-sections = false
relative-imports-order = "furthest-to-closest"
# Override in which order the sections should be output.
section-order = [
  "future",
  "standard-library",
  "third-party",
  "first-party",
  "local-folder"
]

[lint.pydocstyle]
# Use Google-style docstrings.
convention = "google"
# Enable all `pydocstyle` rules, limiting to those that adhere to the
# Google convention via `convention = "google"`, below.
select = ["D"]
# On top of the Google convention, disable `D417`, which requires
# documentation for every function parameter.
ignore = ["D417"]

[lint.pylint]
allow-dunder-method-names = ["__tablename__", "__table_args__"]

```

ruff version: 0.6.8
Python version: 3.10.12
Platform: macOS 14.6.1 arm64


---

_Comment by @dhruvmanila on 2024-10-03 06:19_

The [`select`](https://docs.astral.sh/ruff/settings/#lint_select) and [`ignore` ](https://docs.astral.sh/ruff/settings/#lint_ignore) which is currently present under `[lint.pydocstyle]` should be under `[lint]` instead.

---

_Label `question` added by @dhruvmanila on 2024-10-03 06:19_

---

_Comment by @YaoJusheng on 2024-10-03 06:41_

> The [`select`](https://docs.astral.sh/ruff/settings/#lint_select) and [`ignore` ](https://docs.astral.sh/ruff/settings/#lint_ignore) which is currently present under `[lint.pydocstyle]` should be under `[lint]` instead.

@dhruvmanila  Thanks for the reply. After I moved them to the lint section, they can run normally.


---

_Closed by @YaoJusheng on 2024-10-03 06:41_

---
