```yaml
number: 4415
title: "`PLW3301` auto-fix creates `TypeError`"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2023-05-13T15:47:32Z
updated_at: 2023-05-13T15:48:56Z
url: https://github.com/astral-sh/ruff/issues/4415
synced_at: 2026-01-12T15:54:44Z
```

# `PLW3301` auto-fix creates `TypeError`

---

_@janosh_

This line raises `PLW3301` (Nested `max` calls can be flattened)

```py
max(1, max([1, 2, 3]))
```

and auto-fixes to

```py
max(1, [1, 2, 3])
```

which raises

> TypeError: '>' not supported between instances of 'list' and 'int'

This seems serious and in need of a hotfix.

---

_Comment by @charliermarsh on 2023-05-13 15:48_

This is a duplicate of #4410 and was fixed by #4412.

---

_Closed by @charliermarsh on 2023-05-13 15:48_

---
