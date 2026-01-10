```yaml
number: 10747
title: Remove unused dependencies
type: pull_request
state: merged
author: Boshen
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-unused-deps
created_at: 2024-04-03T07:31:08Z
updated_at: 2024-04-03T09:11:17Z
url: https://github.com/astral-sh/ruff/pull/10747
synced_at: 2026-01-10T22:47:03Z
```

# Remove unused dependencies

---

_Pull request opened by @Boshen on 2024-04-03 07:31_

## Summary

Continuation of #10475, I improved [`cargo shear`](https://github.com/Boshen/cargo-shear) even more.

We can put this in CI once I test it a bit more, given that [ignoring false positives](https://github.com/Boshen/cargo-shear?tab=readme-ov-file#ignore-false-positives) has been implemented.

## Test Plan

`cargo check --all-features --all-targets`


---

_Review requested from @MichaReiser by @Boshen on 2024-04-03 07:31_

---

_Converted to draft by @Boshen on 2024-04-03 07:39_

---

_Comment by @Boshen on 2024-04-03 07:42_

Oh I forgot to pull main ... this gives us only 1 removal.

---

_Marked ready for review by @Boshen on 2024-04-03 07:51_

---

_Comment by @github-actions[bot] on 2024-04-03 08:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2024-04-03 08:56_

Thanks!

---

_Merged by @AlexWaygood on 2024-04-03 08:57_

---

_Closed by @AlexWaygood on 2024-04-03 08:57_

---

_Label `internal` added by @AlexWaygood on 2024-04-03 09:11_

---
