```yaml
number: 19963
title: "[ty] Short-circuit inlayhints request if disabled in settings"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/short-circuit-inlay
created_at: 2025-08-18T08:31:42Z
updated_at: 2025-08-18T10:35:41Z
url: https://github.com/astral-sh/ruff/pull/19963
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Short-circuit inlayhints request if disabled in settings

---

_Pull request opened by @MichaReiser on 2025-08-18 08:31_

## Summary

Not only is it unnecessary to visit the entire AST if all inlay hints are disabled, it also makes it more difficult to
narrow down issues because there's no easy way to disable all inlays (other than disabling them at the editor level).



---

_Label `server` added by @MichaReiser on 2025-08-18 08:31_

---

_Label `ty` added by @MichaReiser on 2025-08-18 08:31_

---

_Review requested from @carljm by @MichaReiser on 2025-08-18 08:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-18 08:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-18 08:31_

---

_Label `server` added by @MichaReiser on 2025-08-18 08:31_

---

_Label `ty` added by @MichaReiser on 2025-08-18 08:31_

---

_Renamed from "[ty] Short-circuit inlayhints request if all are disabled" to "[ty] Short-circuit inlayhints request if disabled in settings" by @MichaReiser on 2025-08-18 08:32_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-18 08:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-18 08:33_

---

_Comment by @github-actions[bot] on 2025-08-18 08:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-18 08:36_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/inlay_hints.rs`:87 on 2025-08-18 08:46_

Can we add a short comment somewhere here stating that if more options are added, it should be considered for `any_enabled`?

---

_@dhruvmanila approved on 2025-08-18 08:46_

Thank you!

---

_Merged by @MichaReiser on 2025-08-18 10:35_

---

_Closed by @MichaReiser on 2025-08-18 10:35_

---

_Branch deleted on 2025-08-18 10:35_

---
