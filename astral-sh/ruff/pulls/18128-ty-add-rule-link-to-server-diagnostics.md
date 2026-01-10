```yaml
number: 18128
title: "[ty] Add rule link to server diagnostics"
type: pull_request
state: merged
author: kiran-4444
labels:
  - server
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: server/add-links-to-documentation-rules
created_at: 2025-05-16T07:35:31Z
updated_at: 2025-05-17T17:28:00Z
url: https://github.com/astral-sh/ruff/pull/18128
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Add rule link to server diagnostics

---

_Pull request opened by @kiran-4444 on 2025-05-16 07:35_

## Summary

Fixes https://github.com/astral-sh/ty/issues/325

Currently, the base URL has been hardcoded. I'd appreciate a better way to do this.

## Test Plan

Tested locally


---

_Review requested from @carljm by @kiran-4444 on 2025-05-16 07:35_

---

_Review requested from @MichaReiser by @kiran-4444 on 2025-05-16 07:35_

---

_Review requested from @AlexWaygood by @kiran-4444 on 2025-05-16 07:35_

---

_Review requested from @sharkdp by @kiran-4444 on 2025-05-16 07:35_

---

_Review requested from @dcreager by @kiran-4444 on 2025-05-16 07:35_

---

_Comment by @github-actions[bot] on 2025-05-16 07:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/diagnostic.rs`:119 on 2025-05-16 08:54_

We shouldn't early return if the URL fails to parse (it would omit the diagnostic). Instead, we should set None (and maybe log a debug message with `tracing::debug`

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/diagnostic.rs`:114 on 2025-05-16 08:55_

`code` always seems to be `Some`... we can remove the `and_then` here

---

_@MichaReiser reviewed on 2025-05-16 08:55_

Nice, 

Would you mind adding a small screenshot as test plan to the PR summary?

---

_Label `server` added by @MichaReiser on 2025-05-16 08:56_

---

_Label `ty` added by @MichaReiser on 2025-05-16 08:56_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-16 08:56_

---

_@InSyncWithFoo reviewed on 2025-05-16 15:37_

---

_Review comment by @InSyncWithFoo on `crates/ty_server/src/server/api/requests/diagnostic.rs`:119 on 2025-05-16 15:37_

Diagnostic IDs should always be valid anchors, no? If so, we can instead add a test to guarantee that this is the case, then use `.expect()`. That would remove the need for logging altogether.

---

_@MichaReiser reviewed on 2025-05-16 16:01_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/diagnostic.rs`:119 on 2025-05-16 16:01_

They should. But I'd rather not panic the entire server just because one id isn't a valid anchor

That made me realise. We should only create a link if the id is a `LintName`

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-16 16:24_

---

_Merged by @MichaReiser on 2025-05-17 17:28_

---

_Closed by @MichaReiser on 2025-05-17 17:28_

---
