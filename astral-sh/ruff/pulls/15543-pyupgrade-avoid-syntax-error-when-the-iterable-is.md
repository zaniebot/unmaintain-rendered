```yaml
number: 15543
title: "[`pyupgrade`] Avoid syntax error when the iterable is an non-parenthesized tuple (`UP028`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: UP028
created_at: 2025-01-17T00:27:12Z
updated_at: 2025-01-17T03:52:58Z
url: https://github.com/astral-sh/ruff/pull/15543
synced_at: 2026-01-10T20:34:00Z
```

# [`pyupgrade`] Avoid syntax error when the iterable is an non-parenthesized tuple (`UP028`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-17 00:27_

## Summary

Resolves #15540.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-17 00:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2025-01-17 01:13_

Thanks, this makes sense to me.

---

_Comment by @charliermarsh on 2025-01-17 01:13_

I think an unparenthesized generator might have a similar problem, but isn't syntactically valid in the `for`, like:

```python
for _ in x for y in z:
    pass
```

---

_Merged by @charliermarsh on 2025-01-17 01:13_

---

_Closed by @charliermarsh on 2025-01-17 01:13_

---

_Branch deleted on 2025-01-17 01:42_

---

_Label `bug` added by @dhruvmanila on 2025-01-17 03:52_

---
