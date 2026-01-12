```yaml
number: 14520
title: "[`ruff`] Handle `attrs`'s `auto_attribs` correctly (`RUF009`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: RUF009
created_at: 2024-11-22T03:16:47Z
updated_at: 2024-11-24T04:33:09Z
url: https://github.com/astral-sh/ruff/pull/14520
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] Handle `attrs`'s `auto_attribs` correctly (`RUF009`)

---

_@InSyncWithFoo_

## Summary

Resolves #14519.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Renamed from "[`ruff`] Handle `attrs`'s auto_attribs` correctly (`RUF009`)" to "[`ruff`] Handle `attrs`'s `auto_attribs` correctly (`RUF009`)" by @InSyncWithFoo on 2024-11-22 03:17_

---

_Comment by @github-actions[bot] on 2024-11-22 03:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-11-24 02:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/helpers.rs`:151 on 2024-11-24 02:40_

Can we use `Truthiness` here?

---

_@charliermarsh approved on 2024-11-24 02:44_

Nicely done, thanks.

---

_Merged by @charliermarsh on 2024-11-24 02:46_

---

_Closed by @charliermarsh on 2024-11-24 02:46_

---

_Label `bug` added by @charliermarsh on 2024-11-24 02:46_

---

_Branch deleted on 2024-11-24 04:33_

---
