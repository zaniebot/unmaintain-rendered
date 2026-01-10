```yaml
number: 17722
title: "[`ruff`] add fix safety section (`RUF028`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: document-fix-safety-invalid-formatter-suppresion-comments
created_at: 2025-04-29T20:16:59Z
updated_at: 2025-04-30T19:06:25Z
url: https://github.com/astral-sh/ruff/pull/17722
synced_at: 2026-01-10T19:03:00Z
```

# [`ruff`] add fix safety section (`RUF028`)

---

_Pull request opened by @VascoSch92 on 2025-04-29 20:16_

The PR add the fix safety section for rule `RUF028` (https://github.com/astral-sh/ruff/issues/15584 )

See also [here](https://github.com/astral-sh/ruff/issues/15584#issuecomment-2820424485) for the reason behind the _unsafe_ of the fix.

---

_Comment by @github-actions[bot] on 2025-04-29 20:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-04-29 20:30_

---

_@ntBre reviewed on 2025-04-29 20:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:56 on 2025-04-29 20:48_

This seems a bit vague to me. I think something closer to Micha's original comment would be more helpful. Maybe something like:

```
This fix is always marked as unsafe because it deletes the invalid suppression comment,
rather than trying to move it to a valid position, which the user more likely intended.
```

What do you think?

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:56 on 2025-04-30 16:45_

```suggestion
/// This fix is always marked as unsafe because it deletes the invalid suppression comment,
/// rather than trying to move it to a valid position, which the user more likely intended.
```

---

_@VascoSch92 reviewed on 2025-04-30 16:45_

---

_@VascoSch92 reviewed on 2025-04-30 16:46_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/ruff/rules/invalid_formatter_suppression_comment.rs`:56 on 2025-04-30 16:46_

@ntBre  It makes sense ;)

Updated!

---

_@ntBre approved on 2025-04-30 19:05_

Thanks!

---

_Merged by @ntBre on 2025-04-30 19:06_

---

_Closed by @ntBre on 2025-04-30 19:06_

---
