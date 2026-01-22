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
updated_at: 2026-01-22T00:10:50Z
url: https://github.com/astral-sh/uv/pull/17643
synced_at: 2026-01-22T01:09:36Z
```

# Warn about PyPy being unmaintained

---

_@konstin_

It seems that PyPy is not being actively developed anymore and is phased out even by numpy (https://github.com/numpy/numpy/issues/30416). There's no official statement from the project, but the numpy issue is from a PyPy developer. I added a warning to avoid users assuming PyPy properly supported and developed Python distribution, and in anticipation of PyPy being eventually, slowly deprecated.

---

_Label `documentation` added by @konstin on 2026-01-21 16:47_

---

_Review comment by @EliteTK on `docs/concepts/python-versions.md`:453 on 2026-01-22 00:04_

```suggestion
    PyPy is [not actively developed anymore](https://github.com/numpy/numpy/issues/30416) and
    only supports Python versions up to 3.11.
```

Flows a little bit better.

---

_Review comment by @EliteTK on `docs/concepts/python-versions.md`:481 on 2026-01-22 00:05_

See above.

---

_@EliteTK approved on 2026-01-22 00:07_

Is it intentional that the note is duplicated?

It feels like it might be, but they're very close together - seems somewhat excessive?

---

_Comment by @zanieb on 2026-01-22 00:10_

Yeah I think maybe let's just note it on the managed distributions section?

---
