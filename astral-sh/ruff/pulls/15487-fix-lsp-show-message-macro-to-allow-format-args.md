```yaml
number: 15487
title: Fix LSP show message macro to allow format args
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/fix-macro
created_at: 2025-01-15T06:32:30Z
updated_at: 2025-01-15T08:13:12Z
url: https://github.com/astral-sh/ruff/pull/15487
synced_at: 2026-01-10T20:34:00Z
```

# Fix LSP show message macro to allow format args

---

_Pull request opened by @dhruvmanila on 2025-01-15 06:32_

## Summary

This PR fixes the `show_*_msg` macros to pass all the tokens instead of just a single token. This allows for using various expressions right in the macro similar to how it would be in `format_args!`.

## Test Plan

`cargo clippy`


---

_Label `internal` added by @dhruvmanila on 2025-01-15 06:32_

---

_Review requested from @carljm by @dhruvmanila on 2025-01-15 06:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-15 06:32_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-01-15 06:32_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-01-15 06:32_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:192 on 2025-01-15 08:01_

```suggestion
                    "Error while resolving settings from workspace {root}. Please refer to the logs for more details.",
                    root = root.display()
                );
```

---

_@MichaReiser approved on 2025-01-15 08:01_

---

_Merged by @dhruvmanila on 2025-01-15 08:11_

---

_Closed by @dhruvmanila on 2025-01-15 08:11_

---

_Branch deleted on 2025-01-15 08:11_

---

_Comment by @github-actions[bot] on 2025-01-15 08:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
