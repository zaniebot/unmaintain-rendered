---
number: 21648
title: PERF401 false negative
type: issue
state: open
author: victorsamun
labels:
  - rule
assignees: []
created_at: 2025-11-26T21:55:40Z
updated_at: 2025-11-27T06:59:40Z
url: https://github.com/astral-sh/ruff/issues/21648
synced_at: 2026-01-07T13:12:16-06:00
---

# PERF401 false negative

---

_Issue opened by @victorsamun on 2025-11-26 21:55_

### Summary

When using unpacking in for-loop variable rule PERF401 does not work.

Wrong behaviour:
```bash
$ cat 1.py && ruff check --select PERF401 1.py
xs = []
for x, y in f():
    xs.append(x + y)
All checks passed!
```

Correct behaviour:
```bash
$ cat 2.py && ruff check --select PERF401 2.py
xs = []
for xy in f():
    xs.append(xy[0] + xy[1])
PERF401 Use a list comprehension to create a transformed list
 --> 2.py:3:5
  |
1 | xs = []
2 | for xy in f():
3 |     xs.append(xy[0] + xy[1])
  |     ^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Replace for loop with list comprehension

Found 1 error.
```


### Version

0.14.6

---

_Label `rule` added by @MichaReiser on 2025-11-27 06:59_

---
