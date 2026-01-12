```yaml
number: 15405
title: "[`flake8-simplify`] Do not emit diagnostics for expressions inside string type annotations (`SIM222`, `SIM223`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: SIM222
created_at: 2025-01-10T16:57:16Z
updated_at: 2025-01-17T12:52:26Z
url: https://github.com/astral-sh/ruff/pull/15405
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-simplify`] Do not emit diagnostics for expressions inside string type annotations (`SIM222`, `SIM223`)

---

_@InSyncWithFoo_

## Summary

Resolves #7127.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-10 17:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2025-01-11 13:36_

I think we should still emit diagnostics here; we just may not want to offer a fix.

---

_Label `bug` added by @dhruvmanila on 2025-01-16 06:30_

---

_@dhruvmanila requested changes on 2025-01-16 06:36_

I agree with Charlie that we should keep highlighting this but avoid providing a fix. As such, this is an invalid type expression.

---

_Comment by @InSyncWithFoo on 2025-01-16 12:48_

`"'spline' or 'linear'"` is not just an invalid type expression; It is a <em>quoted</em> invalid type expression. Such strings are very likely not intended to be type hints and would not benefit from an expression simplification.

It should be up to [`F722`](https://docs.astral.sh/ruff/rules/forward-annotation-syntax-error/), [`F821`](https://docs.astral.sh/ruff/rules/undefined-name/) and other rules as well as type checkers to determine what to do with them.

---

_@dhruvmanila approved on 2025-01-17 06:48_

Related https://github.com/astral-sh/ruff/issues/10586

I thought about this more and I think this should be fine. We've done this kind of fixes before.

---

_Merged by @dhruvmanila on 2025-01-17 06:48_

---

_Closed by @dhruvmanila on 2025-01-17 06:48_

---

_Branch deleted on 2025-01-17 12:52_

---
