```yaml
number: 19353
title: "[ty] Remove `ConnectionInitializer`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/remove-connection-initializer
created_at: 2025-07-15T11:16:03Z
updated_at: 2025-07-15T11:32:46Z
url: https://github.com/astral-sh/ruff/pull/19353
synced_at: 2026-01-12T15:56:37Z
```

# [ty] Remove `ConnectionInitializer`

---

_@dhruvmanila_

## Summary

This PR removes the `ConnectionInitializer` and inlines the `initialize_start` and `initialize_finish` calls.

The main benefit of this is that it will allow us to use [`Connection::memory`](https://docs.rs/lsp-server/latest/lsp_server/struct.Connection.html#method.memory) in the mock server. That method returns two `Connection` where one of them will represent the client side connection and the other will be sent to the `Server::new` call to be used by the server. This way the mock client can send notifications and requests to mimic the editor.

## Test Plan

I tested out the initialization process and checked that the initialized result contains the server capabilities and server info.


---

_Review requested from @carljm by @dhruvmanila on 2025-07-15 11:16_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-15 11:16_

---

_Label `internal` added by @dhruvmanila on 2025-07-15 11:16_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-15 11:16_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-15 11:16_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-15 11:16_

---

_Label `server` added by @dhruvmanila on 2025-07-15 11:16_

---

_Label `ty` added by @dhruvmanila on 2025-07-15 11:16_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-15 11:16_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-15 11:16_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-15 11:16_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-15 11:16_

---

_Comment by @github-actions[bot] on 2025-07-15 11:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-07-15 11:23_

---

_Merged by @dhruvmanila on 2025-07-15 11:32_

---

_Closed by @dhruvmanila on 2025-07-15 11:32_

---

_Branch deleted on 2025-07-15 11:32_

---
