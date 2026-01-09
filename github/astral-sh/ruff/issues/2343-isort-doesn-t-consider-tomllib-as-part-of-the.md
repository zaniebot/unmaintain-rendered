---
number: 2343
title: "Isort doesn't consider tomllib as part of the standard library"
type: issue
state: closed
author: yakMM
labels: []
assignees: []
created_at: 2023-01-30T08:46:59Z
updated_at: 2023-06-19T05:01:09Z
url: https://github.com/astral-sh/ruff/issues/2343
synced_at: 2026-01-07T13:12:14-06:00
---

# Isort doesn't consider tomllib as part of the standard library

---

_Issue opened by @yakMM on 2023-01-30 08:46_

Consider the following: 

```py
import tomllib

import sys


def main():
    print(sys)
    print(tomllib)
```

Formatted with isort:

```py
import sys
import tomllib


def main():
    print(sys)
    print(tomllib)
```

Formatted with `ruff --fix`:

```py
import sys

import tomllib


def main():
    print(sys)
    print(tomllib)
```
Version: `ruff 0.0.237`

---

_Comment by @yakMM on 2023-01-30 08:55_

Isort uses a list of known stdlib modules for each python version: https://github.com/PyCQA/isort/tree/main/isort/stdlibs

While ruff seems to use a static list for every python version: https://github.com/charliermarsh/ruff/blob/main/src/python/sys.rs

Is it worth doing the same as isort here?

---

_Comment by @sbrugman on 2023-01-30 09:05_

Would be good to respect the `target-version` for the imports. For the time being we could include `tomllib` in the existing setup, and keep this issue open to the track version-aware stdlib modules (good issue to get started).

Note that `isort` auto-generates these files [here](https://github.com/PyCQA/isort/blob/main/scripts/mkstdlibs.py).

---

_Referenced in [astral-sh/ruff#2345](../../astral-sh/ruff/pulls/2345.md) on 2023-01-30 10:04_

---

_Comment by @charliermarsh on 2023-01-30 22:55_

Fixed in #2345.

---

_Closed by @charliermarsh on 2023-01-30 22:55_

---

_Comment by @yakMM on 2023-01-31 09:02_

Should another issue be opened to respect the `target-version` for the imports or is supporting the latest stdlib enough?

---

_Referenced in [astral-sh/ruff#2491](../../astral-sh/ruff/pulls/2491.md) on 2023-02-02 19:42_

---

_Comment by @bersbersbers on 2023-06-19 04:35_

I think I am seeing this issue (again) in `ruff 0.0.272`. It wants to separate `sys` and `tomllib` while `isort` wants to keep them together. This is not happening in an empty folder with only a single `bug.py`, but it does happen as soon as I move this `pyproject.toml` next to it:
```toml
[tool.ruff]
select = ["I001"]
```

Output is
```
[2023-06-19][06:35:22][ruff_cli::commands::run][DEBUG] Identified files to lint in: 3.5124ms
[2023-06-19][06:35:22][ruff_cli::diagnostics][DEBUG] Checking: C:\Code\bug.py
[2023-06-19][06:35:22][ruff::rules::isort::categorize][DEBUG] Categorized 'tomllib' as Known(ThirdParty) (NoMatch)
[2023-06-19][06:35:22][ruff::rules::isort::categorize][DEBUG] Categorized 'sys' as Known(StandardLibrary) (KnownStandardLibrary)
[2023-06-19][06:35:22][ruff::rules::isort::categorize][DEBUG] Categorized 'tomllib' as Known(ThirdParty) (NoMatch)
[2023-06-19][06:35:22][ruff::rules::isort::categorize][DEBUG] Categorized 'sys' as Known(StandardLibrary) (KnownStandardLibrary)
[2023-06-19][06:35:22][ruff_cli::commands::run][DEBUG] Checked 1 files in: 8.0006ms
Found 1 error (1 fixed, 0 remaining).
```

---

_Comment by @bersbersbers on 2023-06-19 04:56_

Update: I noticed I still had `requires-python = ">=3.10"` in my `pyproject.toml` - changing that to `>=3.11` fixed the issue. (The fact that this is not assumed by running on Python 3.11.4, even without a `project.toml` file, is probably intended.)

---

_Referenced in [astral-sh/ruff-vscode#465](../../astral-sh/ruff-vscode/issues/465.md) on 2024-05-16 08:33_

---
