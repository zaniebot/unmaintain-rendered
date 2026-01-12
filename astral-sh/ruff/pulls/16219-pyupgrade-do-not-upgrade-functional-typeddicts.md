```yaml
number: 16219
title: "[`pyupgrade`] Do not upgrade functional TypedDicts with private field names to the class-based syntax (`UP013`)"
type: pull_request
state: merged
author: sobolevn
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: UP013-private-names
created_at: 2025-02-17T18:32:22Z
updated_at: 2025-02-18T13:07:07Z
url: https://github.com/astral-sh/ruff/pull/16219
synced_at: 2026-01-12T15:55:54Z
```

# [`pyupgrade`] Do not upgrade functional TypedDicts with private field names to the class-based syntax (`UP013`)

---

_@sobolevn_

## Summary

CPython mangles private names defined in `TypedDict` class body, so it is not recommended to use class form with such names, see https://github.com/python/cpython/issues/129567

But, now `ruff` tries to warn about converting such examples to class form. See https://github.com/graphql-python/graphql-core/pull/234

## Test Plan

I included a test case.


---

_Comment by @github-actions[bot] on 2025-02-17 18:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sobolevn on 2025-02-17 19:14_

CC @AlexWaygood 

---

_Label `rule` added by @ntBre on 2025-02-17 19:44_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-18 06:38_

---

_@AlexWaygood reviewed on 2025-02-18 11:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/convert_typed_dict_functional_to_class.rs`:206 on 2025-02-18 11:39_

I think it would be good to add a comment here explaining why it's not safe to convert TypedDicts with these fields to the class-based syntax. And it might be useful to add a sentence or two to the docs explaining why not all functional TypedDicts can be converted (and that therefore this rule deliberately does not flag them), as well!

---

_@sobolevn reviewed on 2025-02-18 12:04_

---

_Review comment by @sobolevn on `crates/ruff_linter/src/rules/pyupgrade/rules/convert_typed_dict_functional_to_class.rs`:206 on 2025-02-18 12:04_

Done

---

_@AlexWaygood approved on 2025-02-18 13:00_

Thanks!

---

_Renamed from "UP013: do not report functional form with private names" to "[`pyupgrade`] Do not upgrade functional TypedDicts with private names to the class-based syntax (`UP018`)" by @AlexWaygood on 2025-02-18 13:00_

---

_Renamed from "[`pyupgrade`] Do not upgrade functional TypedDicts with private names to the class-based syntax (`UP018`)" to "[`pyupgrade`] Do not upgrade functional TypedDicts with private names to the class-based syntax (`UP013`)" by @AlexWaygood on 2025-02-18 13:01_

---

_Label `bug` added by @AlexWaygood on 2025-02-18 13:01_

---

_Renamed from "[`pyupgrade`] Do not upgrade functional TypedDicts with private names to the class-based syntax (`UP013`)" to "[`pyupgrade`] Do not upgrade functional TypedDicts with private field names to the class-based syntax (`UP013`)" by @AlexWaygood on 2025-02-18 13:01_

---

_Merged by @AlexWaygood on 2025-02-18 13:03_

---

_Closed by @AlexWaygood on 2025-02-18 13:03_

---
