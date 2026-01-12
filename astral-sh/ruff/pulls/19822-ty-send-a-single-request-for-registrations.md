```yaml
number: 19822
title: "[ty] Send a single request for registrations/unregistrations"
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
head: dhruv/register-unregister-capabilities
created_at: 2025-08-08T06:17:56Z
updated_at: 2025-08-08T08:43:38Z
url: https://github.com/astral-sh/ruff/pull/19822
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Send a single request for registrations/unregistrations

---

_@dhruvmanila_

## Summary

This is a small refactor to update the server to send a single request to perform registrations and unregistrations of dynamic capabilities.

## Test Plan

Existing E2E test cases pass, add a new test case to verify multiple registrations.


---

_Review requested from @carljm by @dhruvmanila on 2025-08-08 06:17_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-08 06:17_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-08 06:17_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-08 06:17_

---

_Label `internal` added by @dhruvmanila on 2025-08-08 06:17_

---

_Label `server` added by @dhruvmanila on 2025-08-08 06:17_

---

_Label `ty` added by @dhruvmanila on 2025-08-08 06:17_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-08-08 06:18_

---

_Review request for @carljm removed by @dhruvmanila on 2025-08-08 06:18_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-08-08 06:18_

---

_Comment by @github-actions[bot] on 2025-08-08 06:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 06:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:703 on 2025-08-08 07:45_

Should we error/warn if there's an `unregistration` that isn't present in `self.registrations`?

---

_@MichaReiser approved on 2025-08-08 07:45_

Nice, thank you!

---

_@dhruvmanila reviewed on 2025-08-08 08:09_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:703 on 2025-08-08 08:09_

I don't know if that's disallowed or not as there's nothing like that mentioned in the spec but I think it would be good to have it at the debug level.

---

_Merged by @dhruvmanila on 2025-08-08 08:42_

---

_Closed by @dhruvmanila on 2025-08-08 08:42_

---

_Branch deleted on 2025-08-08 08:42_

---
