```yaml
number: 14612
title: "[red-knot] Update KNOWN_FAILURES"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/update-known-failures
created_at: 2024-11-26T14:48:17Z
updated_at: 2024-11-26T14:56:44Z
url: https://github.com/astral-sh/ruff/pull/14612
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Update KNOWN_FAILURES

---

_Pull request opened by @sharkdp on 2024-11-26 14:48_


## Summary

Remove entry that was prevously fixed in 5a30ec0df6478b98071004a1a9255e876c5d5c32.

## Test Plan

```sh
cargo test -p red_knot_workspace -- --ignored linter_af linter_gz
```

---

_Label `red-knot` added by @sharkdp on 2024-11-26 14:48_

---

_Review requested from @carljm by @sharkdp on 2024-11-26 14:48_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-26 14:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-26 14:48_

---

_@MichaReiser approved on 2024-11-26 14:52_

---

_Comment by @github-actions[bot] on 2024-11-26 14:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-11-26 14:56_

---

_Closed by @sharkdp on 2024-11-26 14:56_

---

_Branch deleted on 2024-11-26 14:56_

---
