```yaml
number: 19210
title: Remove deprecated macOS config file discovery
type: pull_request
state: merged
author: CodeMan62
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: brent/0.13.0
head: code/19145
created_at: 2025-07-08T15:30:41Z
updated_at: 2025-09-05T14:06:25Z
url: https://github.com/astral-sh/ruff/pull/19210
synced_at: 2026-01-12T15:56:34Z
```

# Remove deprecated macOS config file discovery

---

_@CodeMan62_

## Summary
In #11115 we moved from defaulting to $HOME/Library/Application Support to $XDG_HOME on macOS and added a deprecation warning if we find files in the old location. So this PR removes the warning
closes #19145 

## Test Plan
Ran `cargo test`


---

_Comment by @github-actions[bot] on 2025-07-08 15:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.13` by @MichaReiser on 2025-07-08 16:24_

---

_Label `do-not-merge` added by @MichaReiser on 2025-07-08 16:24_

---

_Label `do-not-merge` removed by @MichaReiser on 2025-07-08 16:24_

---

_Label `breaking` added by @MichaReiser on 2025-07-08 16:24_

---

_Label `configuration` added by @MichaReiser on 2025-07-08 16:24_

---

_Converted to draft by @dhruvmanila on 2025-07-10 06:22_

---

_@ntBre approved on 2025-09-05 13:55_

Thank you!

---

_Renamed from "refactor(ruff_workspace): remove macOS deprecation warning " to "Remove deprecated macOS config file discovery" by @ntBre on 2025-09-05 13:56_

---

_Marked ready for review by @ntBre on 2025-09-05 13:56_

---

_Merged by @ntBre on 2025-09-05 14:06_

---

_Closed by @ntBre on 2025-09-05 14:06_

---
