```yaml
number: 17485
title: "[`ruff`] add fix safety section (`RUF027`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fix-safety-missing-fstring-syntax
created_at: 2025-04-19T19:46:52Z
updated_at: 2025-04-26T21:44:00Z
url: https://github.com/astral-sh/ruff/pull/17485
synced_at: 2026-01-12T15:56:02Z
```

# [`ruff`] add fix safety section (`RUF027`)

---

_@VascoSch92_

The PR add the `fix safety` section for rule `RUF027` (#15584 ).

Actually, I have an example of a false positive. Should I include it in the` fix safety` section?

---

_Comment by @github-actions[bot] on 2025-04-19 19:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-04-22 19:18_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:35_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:59 on 2025-04-26 13:32_

```suggestion
/// This fix will always change the behavior of the program and, despite the precautions detailed
/// above, this may be undesired. As such the fix is always marked as unsafe.
```

---

_@dylwil3 requested changes on 2025-04-26 13:33_

Maybe we can adjust the wording a bit. Thanks!

---

_Review requested from @dylwil3 by @VascoSch92 on 2025-04-26 21:09_

---

_@dylwil3 approved on 2025-04-26 21:43_

---

_Merged by @dylwil3 on 2025-04-26 21:43_

---

_Closed by @dylwil3 on 2025-04-26 21:43_

---

_Comment by @dylwil3 on 2025-04-26 21:43_

Thanks!

---
