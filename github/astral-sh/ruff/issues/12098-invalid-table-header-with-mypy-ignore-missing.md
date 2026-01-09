---
number: 12098
title: Invalid table header with mypy ignore missing import
type: issue
state: closed
author: thisi5patrick
labels:
  - question
assignees: []
created_at: 2024-06-29T07:15:06Z
updated_at: 2024-06-29T07:44:15Z
url: https://github.com/astral-sh/ruff/issues/12098
synced_at: 2026-01-07T13:12:15-06:00
---

# Invalid table header with mypy ignore missing import

---

_Issue opened by @thisi5patrick on 2024-06-29 07:15_

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

I have this case where I want to exclude a library from being checked in mypy.
Based on mypy's [documentation](https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-library-stubs-or-py-typed-marker) what you should do is add the library as
```
[mypy-foobar.*]
ignore_missing_imports = True
```

The problem now is that after adding the library `Vision` from `pyobjc-framework-vision` into my `pyproject.toml` as such:
```
[tool.mypy-Vision.*]
ignore_missing_imports = true
```
when running `ruff check .` I get:
```
ruff failed
  Cause: Failed to parse <path>/pyproject.toml
  Cause: TOML parse error at line 79, column 18
   |
79 | [tool.mypy-Vision.*]
   |                  ^
invalid table header
expected `.`, `]`
```

Ruff version `ruff 0.5.0`
Python version `Python 3.12.3`



---

_Comment by @MichaReiser on 2024-06-29 07:39_

Can you try to quote the table header?

```toml
["mypy-foobar.*"]
ignore_missing_imports = true
```


or maybe

```toml
[mypy-foobar."*"]
ignore_missing_imports = true
```


A `*` isn't a valid key according to the toml specification https://toml.io/en/v1.0.0#keys

---

_Comment by @MichaReiser on 2024-06-29 07:44_

There's also a [`pyproject.toml` example](https://mypy.readthedocs.io/en/stable/config_file.html#example-pyproject-toml ) in the mypy documentation. 

I'm closing this issue because it seems mainly about figuring out the right syntax for the mypy configuration in a pyproject.toml (and not mypy.ini)


---

_Closed by @MichaReiser on 2024-06-29 07:44_

---

_Label `question` added by @MichaReiser on 2024-06-29 07:44_

---
