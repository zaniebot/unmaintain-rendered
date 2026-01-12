```yaml
number: 12564
title: "Use `$/logTrace` for server trace logs in Zed and VS Code"
type: pull_request
state: merged
author: osiewicz
labels:
  - server
assignees: []
merged: true
base: main
head: log_trace
created_at: 2024-07-29T09:55:17Z
updated_at: 2024-07-30T11:24:15Z
url: https://github.com/astral-sh/ruff/pull/12564
synced_at: 2026-01-12T15:55:41Z
```

# Use `$/logTrace` for server trace logs in Zed and VS Code

---

_@osiewicz_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This pull request adds support for logging via `$/logTrace` RPC messages. It also enables that code path for when a client is Zed editor (as there's no way for us to generically tell whether a client prefers `$/logTrace` over stderr.

Related to: #12523
/cc @dhruvmanila 

## Test Plan

I've built Ruff from this branch and tested it manually with Zed.


---

_Comment by @github-actions[bot] on 2024-07-29 10:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-29 17:15_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:129 on 2024-07-30 02:33_

nit: can we combine both `starts_with` and `==` into a single check that just uses `starts_with("Zed")`? I think that should work as per the possible display names:

https://github.com/zed-industries/zed/blob/b7c6f3e98eb53eb516cba9bb1724ebbbf780cfd4/crates/release_channel/src/lib.rs#L129-L137

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:91 on 2024-07-30 02:34_

Any specific reason why `ClientSender` is a reference? We can just pass in the value as it's going to be consumed anyway.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/trace.rs`:129 on 2024-07-30 02:38_

And, I think we should also include "Visual Studio Code" here because it also supports `$/logTrace` requests.

---

_@dhruvmanila approved on 2024-07-30 02:38_

---

_Label `server` added by @dhruvmanila on 2024-07-30 02:43_

---

_Comment by @dhruvmanila on 2024-07-30 02:44_

I pushed both the changes and verified in VS Code:
```
2024-07-30 08:12:09.990 [info] [Trace - 8:12:09 AM]    0.021675875s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
```

---

_Renamed from "ruff_server: Add support for LogTrace output" to "Use `$/logTrace` for server trace logs in Zed and VS Code" by @dhruvmanila on 2024-07-30 02:44_

---

_Merged by @dhruvmanila on 2024-07-30 03:02_

---

_Closed by @dhruvmanila on 2024-07-30 03:02_

---

_Review comment by @osiewicz on `crates/ruff_server/src/trace.rs`:91 on 2024-07-30 11:15_

Clippy was just complaining about it in [CI](https://github.com/astral-sh/ruff/actions/runs/10142100234/job/28040714091), no other reason.

---

_@osiewicz reviewed on 2024-07-30 11:15_

---

_@MichaReiser reviewed on 2024-07-30 11:24_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/trace.rs`:129 on 2024-07-30 11:24_

Very interesting that there's no `ClientCapability` indicating whether `logTrace` is supported.

---
