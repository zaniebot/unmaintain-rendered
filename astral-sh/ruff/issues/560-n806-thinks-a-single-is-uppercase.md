```yaml
number: 560
title: "N806 thinks a single `_` is uppercase"
type: issue
state: closed
author: tgross35
labels: []
assignees: []
created_at: 2022-11-03T02:14:48Z
updated_at: 2022-11-03T02:39:24Z
url: https://github.com/astral-sh/ruff/issues/560
synced_at: 2026-01-10T15:56:05Z
```

# N806 thinks a single `_` is uppercase

---

_Issue opened by @tgross35 on 2022-11-03 02:14_

It looks like a single `_` as a variable name gets considered uppercase for N806. I don't think this happens for names that start with an underscore, like `_test`

```shell
root@5d53afc0df87:/home# ruff test_n806.py --select N --no-cache
Found 1 error(s).
test_n806.py:3:8: N806 Variable `_` in function should be lowercase
```

```py
def x():
    t = (1, 2)
    a, _ = t
```

---

_Closed by @charliermarsh on 2022-11-03 02:38_

---

_Comment by @charliermarsh on 2022-11-03 02:39_

Good catch! Going out in [v0.0.97](https://github.com/charliermarsh/ruff/releases/tag/v0.0.97) (building now -- I had another quick fix to include anyway).

---
