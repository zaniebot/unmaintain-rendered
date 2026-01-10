---
number: 4511
title: "0.0.269 seems to have broken ruff's pyupgrade configuration"
type: issue
state: closed
author: Julian
labels: []
assignees: []
created_at: 2023-05-18T21:15:14Z
updated_at: 2023-05-18T21:34:29Z
url: https://github.com/astral-sh/ruff/issues/4511
synced_at: 2026-01-10T01:22:43Z
---

# 0.0.269 seems to have broken ruff's pyupgrade configuration

---

_Issue opened by @Julian on 2023-05-18 21:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Sections called `tool.ruff.pyupgrade` (in a pyproject.toml) seem to cause ruff to crash in 0.0.269.

Specifically, given a trivial project:

```
~/Desktop/bar 
⊙  tree                                                                                                                                                                                                                                               julian@Airm
.
├── foo.py
└── pyproject.toml

1 directory, 2 files

~/Desktop/bar 
⊙  cat foo.py                                                                                                                                                                                                                                         julian@Airm

~/Desktop/bar 
⊙  cat pyproject.toml                                                                                                                                                                                                                                 julian@Airm
[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"

[project]
name = "bar"
description = "bar"
requires-python = ">=3.8"
version = "0.1.0"

[tool.ruff]
line-length = 79
target-version = "py38"
select = ["ANN", "B", "D", "D204", "E", "F", "Q", "SIM", "UP", "W"]

[tool.ruff.pyupgrade]
# We support 3.8 + 3.9
keep-runtime-typing = true
```

ruff 0.0.269 crashes with:

```
⊙  python3.11 -m venv venv; venv/bin/python -m pip install ruff==0.0.269 && venv/bin/ruff check .                                                                                                                                                     julian@Airm
Collecting ruff==0.0.269
  Using cached ruff-0.0.269-py3-none-macosx_10_9_x86_64.macosx_11_0_arm64.macosx_10_9_universal2.whl (10.1 MB)
Installing collected packages: ruff
Successfully installed ruff-0.0.269

[notice] A new release of pip is available: 23.0.1 -> 23.1.2
[notice] To update, run: /Users/julian/Desktop/bar/venv/bin/python -m pip install --upgrade pip
error: TOML parse error at line 16, column 12
   |
16 | [tool.ruff.pyupgrade]
   |            ^^^^^^^^^
unknown field `pyupgrade`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `external`, `fix`, `fix-only`, `fixable`, `format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-arguments`, `isort`, `mccabe`, `pep8-naming`, `pycodestyle`, `pydocstyle`, `pylint`, `per-file-ignores`
```

ruff 0.0.267 runs fine:

```
⊙  python3.11 -m venv venv; venv/bin/python -m pip install ruff==0.0.267 && venv/bin/ruff check .                                                                                                                                                     julian@Airm
Collecting ruff==0.0.267
  Using cached ruff-0.0.267-py3-none-macosx_10_9_x86_64.macosx_11_0_arm64.macosx_10_9_universal2.whl (10.1 MB)
Installing collected packages: ruff
Successfully installed ruff-0.0.267

[notice] A new release of pip is available: 23.0.1 -> 23.1.2
[notice] To update, run: /Users/julian/Desktop/bar/venv/bin/python -m pip install --upgrade pip
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
foo.py:1:1: D100 Missing docstring in public module
Found 1 error.
```

Is there some intended change there or perhaps just an allowlist missing an entry? I also can't tell what possibly could be causing the change from a 2 second look at the last 2 commits, but I'm sure it might make sense to you fine folks.

---

_Comment by @Julian on 2023-05-18 21:17_

Ah I see, this is https://github.com/charliermarsh/ruff/blob/d4c0a41b00b0093dac3bcf79ee329e351a5583b4/BREAKING_CHANGES.md?plain=1#L5

---

_Closed by @Julian on 2023-05-18 21:17_

---

_Comment by @charliermarsh on 2023-05-18 21:30_

Yeah sorry about this -- let me know if you have any questions. It turns out that setting is exactly equivalent to turning off those rules, so we opted to remove it to avoid special-casing.

---

_Comment by @Julian on 2023-05-18 21:34_

I'm going to have a lot of repos to update :D -- but yeah I understand not having multiple mechanisms.

---
