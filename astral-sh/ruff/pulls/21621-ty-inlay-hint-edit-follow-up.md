```yaml
number: 21621
title: "[ty] Inlay Hint edit follow up"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: inlay-hint-follow-up
created_at: 2025-11-24T23:28:34Z
updated_at: 2025-12-27T00:21:10Z
url: https://github.com/astral-sh/ruff/pull/21621
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Inlay Hint edit follow up

---

_Pull request opened by @MatthewMckee4 on 2025-11-24 23:28_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Don't allow edits of some more invalid syntax types.

## Test Plan

Add a test for `x = Literal['a']` (similar) to show we don't allow edits.


---

_Marked ready for review by @MatthewMckee4 on 2025-11-24 23:29_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-24 23:29_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-24 23:29_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-24 23:29_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-24 23:29_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-24 23:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 23:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 23:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-11-24 23:39_

---

_Label `ty` added by @AlexWaygood on 2025-11-24 23:39_

---

_Renamed from "Inlay Hint edit follow up" to "[ty] Inlay Hint edit follow up" by @AlexWaygood on 2025-11-25 00:01_

---

_Review requested from @Gankra by @MichaReiser on 2025-11-25 07:46_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-25 07:46_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-25 07:46_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-25 07:46_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-25 07:46_

---

_@Gankra approved on 2025-11-25 13:55_

Awesome, thanks!

---

_Merged by @Gankra on 2025-11-25 13:56_

---

_Closed by @Gankra on 2025-11-25 13:56_

---

_Branch deleted on 2025-12-27 00:21_

---
