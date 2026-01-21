```yaml
number: 17643
title: Warn about PyPy being unmaintained
type: pull_request
state: open
author: konstin
labels:
  - documentation
assignees: []
base: main
head: konsti/warn-pypy
created_at: 2026-01-21T16:47:04Z
updated_at: 2026-01-21T16:47:04Z
url: https://github.com/astral-sh/uv/pull/17643
synced_at: 2026-01-21T17:04:04Z
```

# Warn about PyPy being unmaintained

---

_@konstin_

It seems that PyPy is not being actively developed anymore and is phased out even by numpy (https://github.com/numpy/numpy/issues/30416). There's no official statement from the project, but the numpy issue is from a PyPy developer. I added a warning to avoid users assuming PyPy properly supported and developed Python distribution, and in anticipation of PyPy being eventually, slowly deprecated.

---

_Label `documentation` added by @konstin on 2026-01-21 16:47_

---
