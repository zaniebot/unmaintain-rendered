```yaml
number: 18152
title: "Only enable `js` feature of `uuid` crate for wasm crates"
type: pull_request
state: merged
author: dsherret
labels:
  - internal
assignees: []
merged: true
base: main
head: fix_ruff_wasm_js_feature
created_at: 2025-05-17T15:29:30Z
updated_at: 2025-05-26T13:39:05Z
url: https://github.com/astral-sh/ruff/pull/18152
synced_at: 2026-01-12T15:56:13Z
```

# Only enable `js` feature of `uuid` crate for wasm crates

---

_@dsherret_

## Summary

This moves the feature enablement from the workspace to only the crates that needs it, which means the other crates don't automatically enable this feature on the `uuid` crate (which causes wasm-bindgen to be pulled in).

## Test Plan

It builds still.


---

_Comment by @MichaReiser on 2025-05-17 15:32_

Hi @dsherret Nice to see you around :)

---

_Label `internal` added by @MichaReiser on 2025-05-17 15:32_

---

_Comment by @MichaReiser on 2025-05-17 15:33_

It looks like cargo-shear doesn't like this :( Can you add it to the ignore list

---

_Comment by @dsherret on 2025-05-17 15:36_

Hey! Yeah, I'll fix it when I get back from lunch and also I seem to have broken the `cargo test (wasm)` workflow (didn't update the ty_wasm crate). I will fix that soon.

Edit: Actually, I was able to fix it quickly I think.

---

_Comment by @github-actions[bot] on 2025-05-17 15:41_

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

_Review requested from @carljm by @dsherret on 2025-05-17 15:41_

---

_Review requested from @MichaReiser by @dsherret on 2025-05-17 15:41_

---

_Review requested from @AlexWaygood by @dsherret on 2025-05-17 15:41_

---

_Review requested from @sharkdp by @dsherret on 2025-05-17 15:41_

---

_Review requested from @dcreager by @dsherret on 2025-05-17 15:41_

---

_Comment by @github-actions[bot] on 2025-05-17 15:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-17 15:54_

---

_Renamed from "Only enable `js` feature of `uuid` crate for `ruff_wasm`" to "Only enable `js` feature of `uuid` crate for wasm crates" by @dsherret on 2025-05-17 18:56_

---

_Review request for @carljm removed by @carljm on 2025-05-20 19:23_

---

_Merged by @MichaReiser on 2025-05-26 09:33_

---

_Closed by @MichaReiser on 2025-05-26 09:33_

---

_Branch deleted on 2025-05-26 13:39_

---
