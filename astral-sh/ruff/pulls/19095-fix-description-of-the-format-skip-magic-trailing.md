```yaml
number: 19095
title: "Fix description of the `format.skip-magic-trailing-comma` example"
type: pull_request
state: merged
author: LeanderCS
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-typo
created_at: 2025-07-02T16:12:24Z
updated_at: 2025-07-03T18:49:47Z
url: https://github.com/astral-sh/ruff/pull/19095
synced_at: 2026-01-10T18:33:12Z
```

# Fix description of the `format.skip-magic-trailing-comma` example

---

_Pull request opened by @LeanderCS on 2025-07-02 16:12_

## Summary

This PR fixes a typo in the docs, where both variants of a config have the same description.


---

_Label `documentation` added by @ntBre on 2025-07-02 16:44_

---

_@ntBre reviewed on 2025-07-02 18:58_

Thanks, this looks good! I think you'll just need to run

```
cargo dev generate-all
```

to update the schema and pass the CI tests. Or I can do that for you if you prefer :)

---

_@ntBre approved on 2025-07-03 14:31_

Thank you!

---

_Renamed from "Fix typo in docs" to "Fix description of the `format.skip-magic-trailing-comma` example" by @ntBre on 2025-07-03 14:33_

---

_Comment by @github-actions[bot] on 2025-07-03 14:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-07-03 14:39_

---

_Closed by @ntBre on 2025-07-03 14:39_

---

_Branch deleted on 2025-07-03 18:49_

---
