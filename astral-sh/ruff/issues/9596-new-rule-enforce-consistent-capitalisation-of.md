```yaml
number: 9596
title: "new rule: enforce consistent capitalisation of `noqa` statements"
type: issue
state: open
author: danieleades
labels:
  - rule
assignees: []
created_at: 2024-01-21T09:55:24Z
updated_at: 2025-01-23T03:27:36Z
url: https://github.com/astral-sh/ruff/issues/9596
synced_at: 2026-01-10T11:09:51Z
```

# new rule: enforce consistent capitalisation of `noqa` statements

---

_Issue opened by @danieleades on 2024-01-21 09:55_

- enforce consistent capitalisation of `noqa` statements
- default to `noqa`, but allow configuring to `NOQA` or `NoQA`

---

_Comment by @danieleades on 2024-01-21 09:55_

see https://github.com/sphinx-doc/sphinx/pull/11903

---

_Label `rule` added by @charliermarsh on 2024-01-21 18:07_

---

_Comment by @danieleades on 2024-01-30 14:29_

@charliermarsh i'd be happy to take a run at implementing this, but i'd need some pointers. I had a quick look at the repo structure and the docs, but it wasn't immediately clear to me. Are there any similarly-implemented rules i should use as an example?

---

_Comment by @InSyncWithFoo on 2025-01-23 03:27_

This would be resolved by #14111, but there was a design problem.

---
