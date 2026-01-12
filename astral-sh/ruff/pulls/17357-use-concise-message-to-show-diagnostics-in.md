```yaml
number: 17357
title: Use concise message to show diagnostics in playground
type: pull_request
state: merged
author: dhruvmanila
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: dhruv/playground-diagnostic
created_at: 2025-04-11T16:38:42Z
updated_at: 2025-04-11T17:14:37Z
url: https://github.com/astral-sh/ruff/pull/17357
synced_at: 2026-01-12T15:56:01Z
```

# Use concise message to show diagnostics in playground

---

_@dhruvmanila_

## Summary

This PR fixes the playground to use the new `concise_diagnostic` method.

## Test plan

**Before:**

<img width="1728" alt="Screenshot 2025-04-11 at 11 37 34 AM" src="https://github.com/user-attachments/assets/cbfcbc52-2e70-4277-9363-ba197711390e" />

**After:**

<img width="1728" alt="Screenshot 2025-04-11 at 11 38 03 AM" src="https://github.com/user-attachments/assets/356ec63c-50d9-49a8-8df4-84000b46fb6d" />



---

_Label `playground` added by @dhruvmanila on 2025-04-11 16:38_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-11 16:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-04-11 16:38_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-11 16:38_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-11 16:38_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-11 16:38_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-11 16:38_

---

_Comment by @github-actions[bot] on 2025-04-11 16:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-11 16:41_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-04-11 16:41_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-11 16:41_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-11 16:41_

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-11 16:41_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:359 on 2025-04-11 16:42_

The idea of `display` is to render the full diagnostic. So I think this should remain `::default` (This is the main difference to `message`)

---

_@MichaReiser approved on 2025-04-11 16:42_

---

_@dhruvmanila reviewed on 2025-04-11 16:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_wasm/src/lib.rs`:359 on 2025-04-11 16:45_

Without this, the message in hover shows only "Reveal type"

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:359 on 2025-04-11 16:52_

Are you sure, because this method isn't used in the playground (and on hover uses types, not diagnostics)

---

_@MichaReiser reviewed on 2025-04-11 16:52_

---

_@dhruvmanila reviewed on 2025-04-11 17:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_wasm/src/lib.rs`:359 on 2025-04-11 17:00_

Oh, you're correct. I must've opened the playground from `main` and didn't notice it's not from localhost

---

_Merged by @dhruvmanila on 2025-04-11 17:14_

---

_Closed by @dhruvmanila on 2025-04-11 17:14_

---

_Branch deleted on 2025-04-11 17:14_

---

_@BurntSushi approved on 2025-04-11 17:14_

LGTM!

---
