```yaml
number: 15678
title: "[`pyflakes`] Visit `TypeVar`'s `default` argument as type expression (`F821`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels: []
assignees: []
base: main
head: F821
created_at: 2025-01-22T22:42:59Z
updated_at: 2025-01-22T23:04:05Z
url: https://github.com/astral-sh/ruff/pull/15678
synced_at: 2026-01-12T15:55:52Z
```

# [`pyflakes`] Visit `TypeVar`'s `default` argument as type expression (`F821`)

---

_@InSyncWithFoo_

## Summary

Resolves #15677.

## Test Plan

`cargo nextest run` and `cargo insta test`.




---

_Comment by @github-actions[bot] on 2025-01-22 22:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-01-22 22:57_

Oh sorry -- I filed #15679 before seeing your comment at https://github.com/astral-sh/ruff/issues/15677#issuecomment-2608419219! I weakly prefer my use of `matches!()` rather than your use of two equality comparisons (it should be _slightly_ more efficient) -- mind if I have this one? :-)

---

_Comment by @InSyncWithFoo on 2025-01-22 22:59_

@AlexWaygood Please, go ahead. It was solely my fault for being a tad too slow!

---

_Closed by @InSyncWithFoo on 2025-01-22 22:59_

---

_Branch deleted on 2025-01-22 23:00_

---

_Comment by @AlexWaygood on 2025-01-22 23:04_

Thanks @InSyncWithFoo!!

---
