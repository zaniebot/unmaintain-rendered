```yaml
number: 12726
title: Use struct instead of type alias for workspace settings index
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/workspace-settings-index
created_at: 2024-08-07T05:06:03Z
updated_at: 2024-08-07T09:36:28Z
url: https://github.com/astral-sh/ruff/pull/12726
synced_at: 2026-01-12T15:55:42Z
```

# Use struct instead of type alias for workspace settings index

---

_@dhruvmanila_

## Summary

Follow-up from https://github.com/astral-sh/ruff/pull/12725, this is just a small refactor to use a wrapper struct instead of type alias for workspace settings index. This avoids the need to have the `register_workspace_settings` as a static method on `Index` and instead is a method on the new struct itself.

---

_Label `internal` added by @dhruvmanila on 2024-08-07 05:06_

---

_Comment by @codspeed-hq[bot] on 2024-08-07 05:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/workspace-settings-index)

### Merging #12726 will **not alter performance**

<sub>Comparing <code>dhruv/workspace-settings-index</code> (8adf81d) with <code>main</code> (7fcfedd)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-07 05:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-07 05:46_

I like it

---

_Merged by @dhruvmanila on 2024-08-07 09:26_

---

_Closed by @dhruvmanila on 2024-08-07 09:26_

---

_Branch deleted on 2024-08-07 09:27_

---
