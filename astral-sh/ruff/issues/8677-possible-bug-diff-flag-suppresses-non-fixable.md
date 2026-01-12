```yaml
number: 8677
title: "Possible bug: Diff flag suppresses non-fixable errors, with no clear way to enable"
type: issue
state: open
author: pnovotnak
labels:
  - cli
assignees: []
created_at: 2023-11-14T16:52:46Z
updated_at: 2024-03-28T11:54:20Z
url: https://github.com/astral-sh/ruff/issues/8677
synced_at: 2026-01-12T15:54:48Z
```

# Possible bug: Diff flag suppresses non-fixable errors, with no clear way to enable

---

_@pnovotnak_

Hello! I discovered what I think is a possible bug with the tool that I wanted to bring to your attention.

I'm using this config:

```
[tool.ruff]
extend-select = ["I", "F"]
target-version = 'py310'
```

My test case is this file (call it `x.py`):

```python


def f(x: "NotImported") -> None:
    pass

```

In CI, it's useful to have the `--diff` flag enabled. However, with the following script:

```bash
#!/bin/bash
set -ex

ruff check --diff .
ruff format --check --diff .
```

No errors are reported when fixes are not available. I think that makes sense, but it's _still_ not reporting errors when `--no-fix-only` is passed.

```
$ ruff check --no-fix-only --diff x.py
$ echo $?
0
$ ruff check x.py
x.py:3:11: F821 Undefined name `NotImported`
Found 1 error.
```

I'm not sure exactly what output should be in the `--no-fix-only --diff` case, but a non-zero exit would be nice, perhaps errors can be printed to stderr?

---

_Renamed from "Possible bug: Diff flag suppresses errors, with no clear way to enable errors" to "Possible bug: Diff flag suppresses non-fixable errors, with no clear way to enable" by @pnovotnak on 2023-11-14 16:53_

---

_Comment by @zanieb on 2023-11-14 17:02_

Hi! Thanks for the well written issue.

This is a duplicate of https://github.com/astral-sh/ruff/issues/8584

Similarly, it may be best resolved by https://github.com/astral-sh/ruff/issues/7352

Perhaps it's possible for us to allow opt-in to this behavior with `--no-fix-only` but I don't think that's a very clear user experience and may prefer #7352 

---

_Label `cli` added by @charliermarsh on 2023-11-15 16:24_

---

_Comment by @pjonsson on 2024-03-28 11:54_

I ran into exactly the same problem, and just like the reporter, I tried adding `--diff --exit-non-zero-on-fix --no-fix-only` to get my exit error code to be non-zero.

@zanieb Isn't it fairly standard behavior that options to the right have precedence over options to the left on the command line, so one can append more arguments (`$cmd --no-x --x --no-x --x --no-x`) to get the desired (`$cmd --no-x`) behavior?

If anyone wants more context, here is an excerpt from the Makefile I'm trying to incorporate Ruff into:
```makefile
Q ?= @

lint.black:
    $(Q)poetry run black --check --diff $(PYTHON_SRC_DIRS)

lint.isort:
    $(Q)poetry run isort --check-only --filter-files --diff $(PYTHON_SRC_DIRS)

ifdef CI_BUILDS_DIR
lint.mypy: MYPY_CI_PARAMS ?= --junit-xml mypy-report.xml --junit-format=per_file
endif
lint.mypy:
    $(Q)poetry run mypy --warn-unused-ignores $(MYPY_CI_PARAMS) $(PYTHON_SRC_DIRS)

<more lint rules that execute various tools>
```


---
