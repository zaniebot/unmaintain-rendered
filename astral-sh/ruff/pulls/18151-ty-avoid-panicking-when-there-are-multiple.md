```yaml
number: 18151
title: "[ty] Avoid panicking when there are multiple workspaces"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/avoid-panic
created_at: 2025-05-17T13:54:27Z
updated_at: 2025-05-20T15:23:25Z
url: https://github.com/astral-sh/ruff/pull/18151
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Avoid panicking when there are multiple workspaces

---

_Pull request opened by @dhruvmanila on 2025-05-17 13:54_

## Summary

This PR updates the language server to avoid panicking when there are multiple workspace folders passed during initialization. The server currently picks up the first workspace folder and provides a warning and a log message.

## Test Plan

<img width="1724" alt="Screenshot 2025-05-17 at 11 43 09" src="https://github.com/user-attachments/assets/1a7ddbc3-198d-4191-a28f-9b69321e8f99" />



---

_Label `server` added by @dhruvmanila on 2025-05-17 13:54_

---

_Label `ty` added by @dhruvmanila on 2025-05-17 13:54_

---

_Comment by @github-actions[bot] on 2025-05-17 15:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-05-17 15:49_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-17 15:49_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-05-17 15:49_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-17 15:49_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-17 15:49_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-17 15:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-17 15:58_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:100 on 2025-05-17 16:18_

Nit
```suggestion
        if workspaces.len() > 1 {
            let first = workspaces.first().unwrap();
        }

        if let [first, _second, ..] = workspaces.as_slice() {}
```

---

_@MichaReiser approved on 2025-05-17 16:20_

---

_@dhruvmanila reviewed on 2025-05-20 15:23_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:100 on 2025-05-20 15:23_

I'm assuming you mean something like the following as I'm not able to follow the suggested diff:
```rs
        let workspaces = if let [first, _second, ..] = workspaces.as_slice() {
            tracing::warn!(
                "Multiple workspaces are not yet supported, using the first workspace: {}",
                &first.0
            );
            show_warn_msg!(
                "Multiple workspaces are not yet supported, using the first workspace: {}",
                &first.0
            );
            vec![first.clone()]
        } else {
            workspaces
        };
```

This is not possible without adding `Clone` to `ClientSettings` which we purposely avoided before.

I'm going to go ahead with the current approach but happy to understand what you meant here.

---

_Merged by @dhruvmanila on 2025-05-20 15:23_

---

_Closed by @dhruvmanila on 2025-05-20 15:23_

---

_Branch deleted on 2025-05-20 15:23_

---
