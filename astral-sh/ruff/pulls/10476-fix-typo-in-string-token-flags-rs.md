```yaml
number: 10476
title: "Fix typo in `string_token_flags.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: string-flags-bug
created_at: 2024-03-19T17:35:15Z
updated_at: 2024-03-19T17:53:07Z
url: https://github.com/astral-sh/ruff/pull/10476
synced_at: 2026-01-12T15:55:32Z
```

# Fix typo in `string_token_flags.rs`

---

_@AlexWaygood_

Currently we produce the wrong prefix if it's a raw bytestring with a capital R.

I ~~spent far too long trying to track this bug down~~ bumped into this while working on a separate PR trying to use these string flags


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-19 17:35_

---

_Label `internal` added by @AlexWaygood on 2024-03-19 17:35_

---

_Merged by @AlexWaygood on 2024-03-19 17:43_

---

_Closed by @AlexWaygood on 2024-03-19 17:43_

---

_Branch deleted on 2024-03-19 17:48_

---

_Comment by @github-actions[bot] on 2024-03-19 17:53_

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
