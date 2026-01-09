---
number: 12963
title: "Feature: Disable unused args check for stub functions"
type: issue
state: closed
author: dargueta
labels: []
assignees: []
created_at: 2024-08-18T13:33:50Z
updated_at: 2024-08-18T23:21:34Z
url: https://github.com/astral-sh/ruff/issues/12963
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature: Disable unused args check for stub functions

---

_Issue opened by @dargueta on 2024-08-18 13:33_

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

Keywords: ARG, ARG001, unused argument

### Feature

It'd be great if we could have some settings to disable the unused argument check for stub and abstract functions, such as one that only throws `NotImplementedError` and does nothing else. The [Flake8 plugin](https://pypi.org/project/flake8-unused-arguments/) has a relevant setting for this.

### To Reproduce

This code:

```py
def foo(a, b):
    raise NotImplementedError
```

Command: `ruff check --isolated --select ARG repro.py `

Output:
```
repro.py:1:9: ARG001 Unused function argument: `a`
  |
1 | def foo(a, b):
  |         ^ ARG001
2 |     raise NotImplementedError
  |

repro.py:1:12: ARG001 Unused function argument: `b`
  |
1 | def foo(a, b):
  |            ^ ARG001
2 |     raise NotImplementedError
  |

Found 2 errors.
```


### Configuration

The output is the same if I don't run it in isolated mode. However, in case you may find it useful, this is  the relevant part of my Ruff configuration in `pyproject.toml`.

```toml
[tool.ruff.lint]
extend-ignore = [
    "COM812",
    "D105",
    "EM",
    "FBT",
    "PLR2004",
    "S311",
    "TRY003",
]
select = ["ALL"]
```

### Platform

* Python: 3.12.3
* Ruff: 0.6.1

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-18 22:53_

---

_Comment by @charliermarsh on 2024-08-18 22:53_

For some reason we already do this for all of the `ARG` rules except `ARG001`.

---

_Referenced in [astral-sh/ruff#12966](../../astral-sh/ruff/pulls/12966.md) on 2024-08-18 22:55_

---

_Closed by @charliermarsh on 2024-08-18 23:21_

---

_Closed by @charliermarsh on 2024-08-18 23:21_

---

_Referenced in [canonical/craft-store#205](../../canonical/craft-store/pulls/205.md) on 2024-09-09 18:46_

---
