```yaml
number: 16000
title: "[red-knot] Resolve `Options` to `Settings`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: micha/settings
created_at: 2025-02-06T18:03:02Z
updated_at: 2025-02-10T14:28:49Z
url: https://github.com/astral-sh/ruff/pull/16000
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Resolve `Options` to `Settings`

---

_@MichaReiser_

## Summary

This PR generalize the idea that we may want to emit diagnostics for 
invalid or incompatible configuration values similar to how we already 
do it for `rules`. 

This PR introduces a new `Settings` struct that is similar to `Options` but, unlike
`Options`, are fields have their default values filled in and they use a representation optimized for reads. 

The diagnostics created during loading the `Settings` are stored on the `Project` so that we can emit them when calling `check`. 

The motivation for this work is that it simplifies adding new settings. That's also why I went ahead and added the `terminal.error-on-warning` setting to demonstrate how new settings are added.

## Test Plan

Existing tests, new CLI test.


---

_Label `cli` added by @MichaReiser on 2025-02-06 18:20_

---

_Label `red-knot` added by @MichaReiser on 2025-02-06 18:20_

---

_Marked ready for review by @MichaReiser on 2025-02-06 18:22_

---

_Review requested from @carljm by @MichaReiser on 2025-02-06 18:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-06 18:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-06 18:22_

---

_@dhruvmanila reviewed on 2025-02-07 09:12_

---

_Review comment by @dhruvmanila on `crates/red_knot_project/src/metadata/settings.rs`:19 on 2025-02-07 09:12_

I believe this will help in converting the mdtest config options into the final resolved settings for the project?

---

_@dhruvmanila approved on 2025-02-07 09:12_

---

_@MichaReiser reviewed on 2025-02-07 10:00_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/settings.rs`:19 on 2025-02-07 10:00_

Probably not because the `mdtest` framework doesn't depend on `red_knot_project`, unless we decide to change that.

---

_Comment by @github-actions[bot] on 2025-02-07 10:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-02-07 10:57_

---

_Review comment by @dhruvmanila on `crates/red_knot_project/src/metadata/settings.rs`:19 on 2025-02-07 10:57_

Ah right.

---

_Merged by @MichaReiser on 2025-02-10 14:28_

---

_Closed by @MichaReiser on 2025-02-10 14:28_

---

_Branch deleted on 2025-02-10 14:28_

---
