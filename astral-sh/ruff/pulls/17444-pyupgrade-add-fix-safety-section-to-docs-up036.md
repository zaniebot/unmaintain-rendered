```yaml
number: 17444
title: "[`pyupgrade`] Add fix safety section to docs (`UP036`)"
type: pull_request
state: merged
author: Kalmaegi
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc_fix_safety_for_outdated_version_block
created_at: 2025-04-17T08:37:56Z
updated_at: 2025-04-18T02:45:54Z
url: https://github.com/astral-sh/ruff/pull/17444
synced_at: 2026-01-12T15:56:02Z
```

# [`pyupgrade`] Add fix safety section to docs (`UP036`)

---

_@Kalmaegi_


## Summary

add fix safety section to outdated_version_block, for #15584 

---

_Assigned to @ntBre by @ntBre on 2025-04-17 18:28_

---

_Label `documentation` added by @ntBre on 2025-04-17 18:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:50 on 2025-04-17 18:31_

```suggestion
/// comments, and annotations within unreachable version blocks.
```

I think we could drop this last part, or possibly reword it. These should _only_ be removed if they are never executed, not *even* if they are. But that's pretty much the rule description, so we could just delete this part.

---

_@ntBre approved on 2025-04-17 18:32_

Thank you! One suggestion here, but it looks good otherwise!

---

_Comment by @github-actions[bot] on 2025-04-17 18:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Kalmaegi on 2025-04-18 01:56_

> Thank you! One suggestion here, but it looks good otherwise!

Indeed, that sentence is somewhat redundant. Thank you very much for your assistance!

---

_Renamed from "add fix safety section to outdated_version_block docs" to "[`pyupgrade`] Add fix safety section to docs (`UP036`)" by @ntBre on 2025-04-18 02:45_

---

_Merged by @ntBre on 2025-04-18 02:45_

---

_Closed by @ntBre on 2025-04-18 02:45_

---
