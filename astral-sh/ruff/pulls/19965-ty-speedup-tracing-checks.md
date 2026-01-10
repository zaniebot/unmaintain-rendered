```yaml
number: 19965
title: "[ty] Speedup tracing checks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/logging
created_at: 2025-08-18T10:29:22Z
updated_at: 2025-08-18T10:56:07Z
url: https://github.com/astral-sh/ruff/pull/19965
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Speedup tracing checks

---

_Pull request opened by @MichaReiser on 2025-08-18 10:29_

## Summary

Overriding `callsite_enabled` in the filter allows tracing to short-circuit many `tracing` calls that are known to always be off by leverging caching.

The implementation follows the recommended implementation of the `Filter` trait if the filtering isn't based on any dynamic data (which isn't the case for us)


## Test Plan

Tested that the server still sends logs for ruff/ty tracing calls


---

_Review requested from @carljm by @MichaReiser on 2025-08-18 10:29_

---

_Label `server` added by @MichaReiser on 2025-08-18 10:29_

---

_Label `ty` added by @MichaReiser on 2025-08-18 10:29_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-18 10:29_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-18 10:29_

---

_Comment by @github-actions[bot] on 2025-08-18 10:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@sharkdp approved on 2025-08-18 10:37_

Thank you

---

_Merged by @MichaReiser on 2025-08-18 10:56_

---

_Closed by @MichaReiser on 2025-08-18 10:56_

---

_Branch deleted on 2025-08-18 10:56_

---
