```yaml
number: 20989
title: "[`ty`] Add capabilities check for `clear_diagnostics`"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: capabilities-check-for-clear_diagnostics
created_at: 2025-10-20T11:08:01Z
updated_at: 2025-10-20T17:46:53Z
url: https://github.com/astral-sh/ruff/pull/20989
synced_at: 2026-01-12T15:57:14Z
```

# [`ty`] Add capabilities check for `clear_diagnostics`

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20784

## Test Plan

<!-- How was it tested? -->

I have tested it with VSCode.

![output](https://github.com/user-attachments/assets/b0ebf6db-16d4-4b67-8abf-50efed9a141d)


---

_Review requested from @carljm by @TaKO8Ki on 2025-10-20 11:08_

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-20 11:08_

---

_Review requested from @sharkdp by @TaKO8Ki on 2025-10-20 11:08_

---

_Review requested from @dcreager by @TaKO8Ki on 2025-10-20 11:08_

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-20 11:09_

---

_@TaKO8Ki reviewed on 2025-10-20 11:09_

---

_Review comment by @TaKO8Ki on `crates/ty_server/src/server/api/diagnostics.rs`:126 on 2025-10-20 11:09_

The same handling as:

https://github.com/astral-sh/ruff/blob/de9ecd96d61b3760f0636f9a2e8494ea08def61b/crates/ty_server/src/server/api/diagnostics.rs#L147-L149

---

_Label `server` added by @MichaReiser on 2025-10-20 11:31_

---

_Label `ty` added by @MichaReiser on 2025-10-20 11:31_

---

_Comment by @MichaReiser on 2025-10-20 11:32_

Would you mind testing this change with VS code? Specifically, that the diagnostics "disappear" after closing the corresponding file.

---

_Comment by @TaKO8Ki on 2025-10-20 13:54_

@MichaReiser I have confirmed it and uploaded a GIF.

---

_@MichaReiser approved on 2025-10-20 13:58_

Perfect, thank you

---

_Comment by @TaKO8Ki on 2025-10-20 14:06_

Are the self hosted runners unstable now?

---

_Comment by @MichaReiser on 2025-10-20 14:32_

Yeah, they're affected by the AWS outage https://status.depot.dev/

---

_Comment by @github-actions[bot] on 2025-10-20 17:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 17:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @MichaReiser on 2025-10-20 17:31_

---

_Closed by @MichaReiser on 2025-10-20 17:31_

---

_Branch deleted on 2025-10-20 17:46_

---
