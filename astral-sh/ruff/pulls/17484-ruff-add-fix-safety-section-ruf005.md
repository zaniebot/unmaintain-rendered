```yaml
number: 17484
title: "[`ruff`] add fix safety section (`RUF005`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fix-safety-collection-literal-conc
created_at: 2025-04-19T19:34:23Z
updated_at: 2025-04-26T21:43:03Z
url: https://github.com/astral-sh/ruff/pull/17484
synced_at: 2026-01-10T19:33:02Z
```

# [`ruff`] add fix safety section (`RUF005`)

---

_Pull request opened by @VascoSch92 on 2025-04-19 19:34_

The PR add the `fix safety` section for rule `RUF005` (#15584 ).

---

_Comment by @github-actions[bot] on 2025-04-19 19:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-04-22 19:19_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:35_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs`:38 on 2025-04-26 13:24_

```suggestion
/// The fix is always marked as unsafe because the `+` operator uses the `__add__` magic method and
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs`:39 on 2025-04-26 13:25_

```suggestion
/// `*`-unpacking uses the `__iter__` magic method. Both of these could have custom
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/collection_literal_concatenation.rs`:40 on 2025-04-26 13:26_

```suggestion
/// implementations, causing the fix to change program behaviour.
```

---

_@dylwil3 requested changes on 2025-04-26 13:26_

---

_Comment by @VascoSch92 on 2025-04-26 21:08_

Hey @dylwil3 
updated with your suggestion ;-) Thanks

---

_@dylwil3 approved on 2025-04-26 21:42_

Thank you!

---

_Merged by @dylwil3 on 2025-04-26 21:43_

---

_Closed by @dylwil3 on 2025-04-26 21:43_

---
