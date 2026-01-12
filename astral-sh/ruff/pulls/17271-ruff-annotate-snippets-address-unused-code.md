```yaml
number: 17271
title: "ruff_annotate_snippets: address unused code warnings"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/suppress-windows-warnings
created_at: 2025-04-07T12:10:48Z
updated_at: 2025-04-07T12:24:09Z
url: https://github.com/astral-sh/ruff/pull/17271
synced_at: 2026-01-12T15:56:01Z
```

# ruff_annotate_snippets: address unused code warnings

---

_@BurntSushi_

Fixes #17230


---

_Comment by @BurntSushi on 2025-04-07 12:12_

This first attempt tries to just remove the `#[cfg(not(windows))]` from the `main` function of the test fixture. This will tell us whether it is superfluous or why it was put there in the first place.

If this doesn't work, I'm inclined to just add `#[allow(dead_code)]` in order to keep the diff between us and upstream small.

---

_Label `internal` added by @MichaReiser on 2025-04-07 12:17_

---

_@MichaReiser approved on 2025-04-07 12:17_

---

_Comment by @github-actions[bot] on 2025-04-07 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @BurntSushi on 2025-04-07 12:23_

Confirmed that all warnings are gone: https://github.com/astral-sh/ruff/actions/runs/14308815650/job/40098718346?pr=17271

---

_Merged by @BurntSushi on 2025-04-07 12:24_

---

_Closed by @BurntSushi on 2025-04-07 12:24_

---

_Branch deleted on 2025-04-07 12:24_

---
