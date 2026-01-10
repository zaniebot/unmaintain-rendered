```yaml
number: 18308
title: "[ty] Move diagnostics API for the server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: dhruv/move-diagnostics-api
created_at: 2025-05-26T04:11:29Z
updated_at: 2025-05-26T04:16:39Z
url: https://github.com/astral-sh/ruff/pull/18308
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Move diagnostics API for the server

---

_Pull request opened by @dhruvmanila on 2025-05-26 04:11_

## Summary

This PR moves the diagnostics API for the language server out from the request handler module to the diagnostics API module.

This is in preparation to add support for publishing diagnostics.


---

_Label `internal` added by @dhruvmanila on 2025-05-26 04:11_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-26 04:11_

---

_Label `server` added by @dhruvmanila on 2025-05-26 04:11_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-05-26 04:11_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-26 04:11_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-26 04:11_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-26 04:11_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-05-26 04:11_

---

_Review request for @carljm removed by @dhruvmanila on 2025-05-26 04:11_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-05-26 04:11_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-05-26 04:11_

---

_@dhruvmanila reviewed on 2025-05-26 04:12_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/diagnostic.rs`:32 on 2025-05-26 04:12_

Switched this to be consistent with how we pass in the `Db` everywhere.

---

_Merged by @dhruvmanila on 2025-05-26 04:16_

---

_Closed by @dhruvmanila on 2025-05-26 04:16_

---

_Branch deleted on 2025-05-26 04:16_

---
