```yaml
number: 15141
title: "[`flake8-pie`] Allow `cast(SomeType, ...)` (`PIE796`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: PIE796
created_at: 2024-12-26T00:05:15Z
updated_at: 2024-12-30T15:06:28Z
url: https://github.com/astral-sh/ruff/pull/15141
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-pie`] Allow `cast(SomeType, ...)` (`PIE796`)

---

_@InSyncWithFoo_

## Summary

Resolves #15132.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-26 00:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pie/rules/non_unique_enums.rs`:97 on 2024-12-26 09:42_

I'd follow the terminology used in the [mypy blog](https://mypy-lang.blogspot.com/2024/12/mypy-114-released.html): `is_enum_member_with_unknown_value_in_stub`. That means I'd also move the `enum.auto` handling out of this function, but we can then inline `is_casted_ellipsis`

---

_@MichaReiser reviewed on 2024-12-26 09:43_

Thanks. This overall looks good to me. I've a small suggestion on how to change the code.

---

_Label `rule` added by @MichaReiser on 2024-12-26 09:44_

---

_@MichaReiser approved on 2024-12-30 14:52_

Thanks

---

_Merged by @MichaReiser on 2024-12-30 14:52_

---

_Closed by @MichaReiser on 2024-12-30 14:52_

---

_Branch deleted on 2024-12-30 15:06_

---
