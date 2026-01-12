```yaml
number: 16262
title: Handle requests received after shutdown message
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/handle-shutdown
created_at: 2025-02-19T17:38:00Z
updated_at: 2025-02-22T05:50:44Z
url: https://github.com/astral-sh/ruff/pull/16262
synced_at: 2026-01-12T15:55:54Z
```

# Handle requests received after shutdown message

---

_@dhruvmanila_

## Summary

This PR should help in https://github.com/astral-sh/ruff-vscode/issues/676.

There are two issues that this is trying to fix all related to the way shutdown should happen as per the protocol:
1. After the server handled the [shutdown request](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#shutdown) and while waiting for the exit notification:
	
	> If a server receives requests after a shutdown request those requests should error with `InvalidRequest`.
    
    But, we raised an error and exited. This PR fixes it by entering a loop which responds to any request during this period with `InvalidRequest`

2. If the server received an [exit notification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#exit) but the shutdown request was never received, the server handled that by logging and exiting with success but as per the spec:

	> The server should exit with success code 0 if the shutdown request has been received before; otherwise with error code 1.

    So, this PR fixes that as well by raising an error in this case.

Closes: https://github.com/astral-sh/ruff-vscode/issues/676

## Test Plan

I'm not sure how to go about testing this without using a mock server.


---

_Label `bug` added by @dhruvmanila on 2025-02-19 17:38_

---

_Label `server` added by @dhruvmanila on 2025-02-19 17:38_

---

_Comment by @github-actions[bot] on 2025-02-19 17:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2025-02-20 09:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-20 09:48_

---

_@dhruvmanila reviewed on 2025-02-20 09:49_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/connection.rs`:128 on 2025-02-20 09:49_

Using `anyhow::bail` as a way to exit the server with failure but we could return an explicit `ExitStatus` instead as well

---

_@dhruvmanila reviewed on 2025-02-20 09:51_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/connection.rs`:95 on 2025-02-20 09:51_

I'm not really a big fan of the loop here, but I also don't think it's likely that the infinite loop will happen because 2 branches below exits the loop while the one responds to the client.

---

_@MichaReiser reviewed on 2025-02-20 10:47_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/connection.rs`:115 on 2025-02-20 10:47_

Should we also do some server side logging to get visibility if this is happening or do you consider the extension logging to be sufficient?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/connection.rs`:118 on 2025-02-20 10:48_

The only other variant seems to be `Notification`. I think we should just discard the notification and log that the server did so.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/connection.rs`:95 on 2025-02-20 10:48_

I don't mind the loop

---

_@MichaReiser approved on 2025-02-20 10:48_

---

_@dhruvmanila reviewed on 2025-02-20 10:48_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/connection.rs`:115 on 2025-02-20 10:48_

Yes, I'll add it

---

_@dhruvmanila reviewed on 2025-02-20 11:01_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/connection.rs`:118 on 2025-02-20 11:01_

Done

---

_Merged by @dhruvmanila on 2025-02-20 11:10_

---

_Closed by @dhruvmanila on 2025-02-20 11:10_

---

_Branch deleted on 2025-02-20 11:10_

---
