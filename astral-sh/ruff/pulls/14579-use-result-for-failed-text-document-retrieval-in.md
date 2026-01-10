```yaml
number: 14579
title: "Use `Result` for failed text document retrieval in LSP requests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: dhruv/single-document-error
created_at: 2024-11-25T03:54:03Z
updated_at: 2024-11-25T09:44:32Z
url: https://github.com/astral-sh/ruff/pull/14579
synced_at: 2026-01-10T20:50:57Z
```

# Use `Result` for failed text document retrieval in LSP requests

---

_Pull request opened by @dhruvmanila on 2024-11-25 03:54_

## Summary

Ref: https://github.com/astral-sh/ruff-vscode/issues/644#issuecomment-2496588452

## Test Plan

Not sure how to test this as this is mainly to get more context on the panic that the server is raising.


---

_Label `internal` added by @dhruvmanila on 2024-11-25 03:54_

---

_Comment by @github-actions[bot] on 2024-11-25 04:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 07:34_

I never fully understood this in the LSP but could we propagate the error instead of unwrapping here?

```suggestion
        .context("Failed to get text document for the format request"))
        .unwrap();
```

I think that should be sufficient. `anyhow` preserves the entire error-chain

---

_@MichaReiser approved on 2024-11-25 07:35_

This is great. 

We should consider propagating the error instead of unwrapping, to prevent the LSP from crashing

---

_Label `server` added by @MichaReiser on 2024-11-25 07:35_

---

_@dhruvmanila reviewed on 2024-11-25 07:57_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 07:57_

Yeah, I thought about doing this but then we would still have the problem that the message will only be logged if the user has turned on server messages via `ruff.trace.server: "messages"`:

https://github.com/astral-sh/ruff/blob/4ce0c42dac09039e86311fdb2da5558947e18188/crates/ruff_server/src/server/api.rs#L197-L214

---

_@dhruvmanila reviewed on 2024-11-25 07:58_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 07:58_

I think I'll want to club the change that you're suggesting along with the logging infrastructure change that I'm planning for later.

---

_@MichaReiser reviewed on 2024-11-25 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:02_

That makes sense. I would still prefer propagating because there's a workaround to get logs, but there's no workaround for a crashing ruff server. 

---

_@dhruvmanila reviewed on 2024-11-25 08:05_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:05_

My main concern here is that the user will only see the notification but no log messages. Then, the user would need to turn on server messages which would restart the server and will loose the state in which the panic occurs. So, the user would need to wait until the next time the error occurs to provide us the logs.

---

_@dhruvmanila reviewed on 2024-11-25 08:40_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:40_

Using anyhow's `Context` requires cloning the `Url` because I can't use the reference because of lifetime issues.

---

_@MichaReiser reviewed on 2024-11-25 08:42_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:42_

Can you use `with_context`

---

_@dhruvmanila reviewed on 2024-11-25 08:45_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:45_

Same issue:

```
error[E0521]: borrowed data escapes outside of function
  --> crates/ruff_server/src/server/api/requests/format.rs:65:25
   |
64 |   pub(super) fn format_document(snapshot: &DocumentSnapshot) -> Result<super::FormatResponse> {
   |                                 --------  - let's call the lifetime of this reference `'1`
   |                                 |
   |                                 `snapshot` is a reference that is only valid in the function body
65 |       let text_document = snapshot
   |  _________________________^
66 | |         .query()
   | |                ^
   | |                |
   | |________________`snapshot` escapes the function body here
   |                  argument requires that `'1` must outlive `'static`

error[E0521]: borrowed data escapes outside of function
  --> crates/ruff_server/src/server/api/requests/format_range.rs:33:25
   |
30 |       snapshot: &DocumentSnapshot,
   |       --------  - let's call the lifetime of this reference `'1`
   |       |
   |       `snapshot` is a reference that is only valid in the function body
...
33 |       let text_document = snapshot
   |  _________________________^
34 | |         .query()
   | |                ^
   | |                |
   | |________________`snapshot` escapes the function body here
   |                  argument requires that `'1` must outlive `'static`

error[E0521]: borrowed data escapes outside of function
  --> crates/ruff_server/src/server/api/requests/hover.rs:34:20
   |
30 |       snapshot: &DocumentSnapshot,
   |       --------  - let's call the lifetime of this reference `'1`
   |       |
   |       `snapshot` is a reference that is only valid in the function body
...
34 |       let document = snapshot
   |  ____________________^
35 | |         .query()
   | |                ^
   | |                |
   | |________________`snapshot` escapes the function body here
   |                  argument requires that `'1` must outlive `'static`

For more information about this error, try `rustc --explain E0521`.
error: could not compile `ruff_server` (lib) due to 3 previous errors
warning: build failed, waiting for other jobs to finish...
error: could not compile `ruff_server` (lib test) due to 3 previous errors
```

---

_@MichaReiser reviewed on 2024-11-25 08:58_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:68 on 2024-11-25 08:58_

Oh I see now. I think cloning is fine. It only happens in the error case and it's uncommon that errors have lifetime bounds (e.g. it prevents users from propagating errors, as you've seen here)

---

_Merged by @dhruvmanila on 2024-11-25 09:44_

---

_Closed by @dhruvmanila on 2024-11-25 09:44_

---

_Branch deleted on 2024-11-25 09:44_

---
