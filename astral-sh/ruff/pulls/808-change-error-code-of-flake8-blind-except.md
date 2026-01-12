```yaml
number: 808
title: Change error code of flake8-blind-except
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: change-code-flake8-blind-except
created_at: 2022-11-18T18:14:31Z
updated_at: 2022-12-01T17:15:21Z
url: https://github.com/astral-sh/ruff/pull/808
synced_at: 2026-01-12T15:55:05Z
```

# Change error code of flake8-blind-except

---

_@JonathanPlasse_

_No description provided._

---

_Comment by @JonathanPlasse on 2022-11-18 18:15_

When using `pre-commit`, I got this error for the file `BLE.py`
```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

error unknown field `mccabe`, expected one of `dummy-variable-rgx`, `exclude`, `extend-exclude`, `extend-ignore`, `extend-select`, `fix`, `ignore`, `line-length`, `select`, `src`, `target-version`, `flake8-annotations`, `flake8-bugbear`, `flake8-quotes`, `flake8-tidy-imports`, `isort`, `pep8-naming`, `per-file-ignores` for key `tool.ruff` at line 45 column 1

Validate pyproject.toml..............................(no files to check)Skipped
```
To reproduce just create a new Python file in `resources/test/fixtures` and commit it.

---

_Comment by @edgarrmondragon on 2022-11-18 18:29_

@JonathanPlasse the latest pre-commit hook doesn't yet support the `mccabe` settings and you may have a `pyproject.toml` that tries to set them.

---

_Merged by @charliermarsh on 2022-11-18 18:30_

---

_Closed by @charliermarsh on 2022-11-18 18:30_

---

_Comment by @charliermarsh on 2022-11-18 18:31_

I'll cut a new pre-commit release as soon as [v0.0.127](https://github.com/charliermarsh/ruff/releases/tag/v0.0.127) is up.

---

_Branch deleted on 2022-12-01 17:15_

---
