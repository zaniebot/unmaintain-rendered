```yaml
number: 21019
title: "[ty] Timeout based workspace diagnostic progress reports"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-diagnostics-time-based-progress
created_at: 2025-10-21T14:31:46Z
updated_at: 2025-10-24T07:06:21Z
url: https://github.com/astral-sh/ruff/pull/21019
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Timeout based workspace diagnostic progress reports

---

_@MichaReiser_

## Summary

This PR changes the workspace diagnostic's progress report (how many files were checked) from sending
a report for every 100th file to sending a report every 50ms. It makes ty feel snappier. 

The only motivation for having a timeout in the first place is to not overwhelm the client
(and not spend more time serializing messages than checking files ;) )

## Test Plan


https://github.com/user-attachments/assets/0c7513bb-8280-4196-9ca0-60fc6517a1b1



---

_Label `server` added by @MichaReiser on 2025-10-21 14:31_

---

_Label `ty` added by @MichaReiser on 2025-10-21 14:31_

---

_Label `server` added by @MichaReiser on 2025-10-21 14:31_

---

_Label `ty` added by @MichaReiser on 2025-10-21 14:31_

---

_Marked ready for review by @MichaReiser on 2025-10-21 14:33_

---

_Review requested from @carljm by @MichaReiser on 2025-10-21 14:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-21 14:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-21 14:33_

---

_Comment by @github-actions[bot] on 2025-10-21 14:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 14:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-23 08:12_

---

_Review comment by @dcreager on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:202 on 2025-10-23 13:32_

nit: Can we import `std::sync::Mutex` instead of fully qualifying here and below?

---

_Review comment by @dcreager on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:245 on 2025-10-23 13:37_

very optional nit: For this use case, it doesn't matter all that much, but this will cause the report times to creep by a bit, instead of being precisely every 50ms.


```suggestion
            state.last_response_sent += Duration::from_millis(50);
```

(That does make the name of the field slightly incorrect, which you could fix by calling it `next_report_time` instead and comparing it against `now`)

---

_@dcreager approved on 2025-10-23 13:37_

---

_@MichaReiser reviewed on 2025-10-24 06:54_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:245 on 2025-10-24 06:54_

This can also lead to drifts if `reprot_file` isn't called for a long time because `Duration::from_millis` might be shorter than the time between two report_file calls. If that happens, then we'll constantly be "behind" and send a report on every future `report_file` call until it eventually catches up again with `now`.

---

_Merged by @MichaReiser on 2025-10-24 07:06_

---

_Closed by @MichaReiser on 2025-10-24 07:06_

---

_Branch deleted on 2025-10-24 07:06_

---
