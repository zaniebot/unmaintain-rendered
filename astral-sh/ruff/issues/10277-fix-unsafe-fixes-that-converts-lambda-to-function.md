```yaml
number: 10277
title: "`--fix --unsafe-fixes` that converts lambda to function (rule E731) removes underscores from numbers."
type: issue
state: closed
author: dwreeves
labels:
  - fixes
  - parser
assignees: []
created_at: 2024-03-07T16:19:57Z
updated_at: 2024-12-23T09:29:48Z
url: https://github.com/astral-sh/ruff/issues/10277
synced_at: 2026-01-10T11:09:52Z
```

# `--fix --unsafe-fixes` that converts lambda to function (rule E731) removes underscores from numbers.

---

_Issue opened by @dwreeves on 2024-03-07 16:19_

Minimal reproduction:

```shell
echo 'at_least_one_million = lambda _: _ >= 1_000_000' > foo.py
ruff foo.py --unsafe-fixes --fix
cat foo.py
```

File:

```python
at_least_one_million = lambda _: _ >= 1_000_000
```

After fix:

```python
def at_least_one_million(_):
    return _ >= 1000000
```

Expected behavior was that it would preserve my underscores (unless another linting rule disables underscores in the number), i.e. something like this:

```python
def at_least_one_million(_):
    return _ >= 1_000_000
```

---

_Label `fixes` added by @charliermarsh on 2024-03-07 21:57_

---

_Comment by @zanieb on 2024-03-11 16:57_

@charliermarsh do we have support for retaining these in general or would we need to add that core functionality?

---

_Comment by @charliermarsh on 2024-03-11 16:58_

@zanieb - We don't retain them in the parser / AST, only the resolved number.

---

_Label `parser` added by @zanieb on 2024-03-11 21:40_

---

_Closed by @MichaReiser on 2024-12-23 09:29_

---
