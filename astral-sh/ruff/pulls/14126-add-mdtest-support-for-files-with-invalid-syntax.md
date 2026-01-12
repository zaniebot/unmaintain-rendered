```yaml
number: 14126
title: Add mdtest support for files with invalid syntax
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/mdtest-invalid-syntax
created_at: 2024-11-06T09:45:55Z
updated_at: 2024-11-06T11:25:54Z
url: https://github.com/astral-sh/ruff/pull/14126
synced_at: 2026-01-12T15:55:46Z
```

# Add mdtest support for files with invalid syntax

---

_@MichaReiser_


## Summary

This PR adds support for mdtests that contain syntax errors. 

Files containing syntax errors must match the `invalid_(+._)syntax` pattern or blackendocs will yell at you that it can't parse the code.

## Test Plan

I ported the exception handler test


---

_Review requested from @carljm by @MichaReiser on 2024-11-06 09:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-06 09:45_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-06 09:45_

---

_Label `testing` added by @MichaReiser on 2024-11-06 09:46_

---

_Label `red-knot` added by @MichaReiser on 2024-11-06 09:46_

---

_@MichaReiser reviewed on 2024-11-06 09:47_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:12 on 2024-11-06 09:47_

@carljm the trait doesn't implement `Ranged` because I can't implement `Ranged` for `ParseError` (orphan rule). We could consider implementing it for `ParseError` but the benefit seemed marginal.

---

_Comment by @github-actions[bot] on 2024-11-06 10:15_

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

_@dhruvmanila approved on 2024-11-06 11:09_

---

_Merged by @MichaReiser on 2024-11-06 11:25_

---

_Closed by @MichaReiser on 2024-11-06 11:25_

---

_Branch deleted on 2024-11-06 11:25_

---
