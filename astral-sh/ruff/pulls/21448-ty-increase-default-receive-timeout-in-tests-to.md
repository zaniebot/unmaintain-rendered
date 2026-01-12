```yaml
number: 21448
title: "[ty] Increase default receive timeout in tests to 10s"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/increase-receive-timeout
created_at: 2025-11-14T11:34:22Z
updated_at: 2025-11-14T12:15:24Z
url: https://github.com/astral-sh/ruff/pull/21448
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Increase default receive timeout in tests to 10s

---

_@MichaReiser_

## Summary

Increase the default timeout for `receive` to 10s because tests on CI can sometimes take a bit longer. 

A longer timeout shouldn't "hurt" as we only use receive in places where we expect a response, notification, or request. 
That means, we should only hit this timeout when the test is likely to error anyway. 

The main downside of this is that it now takes 10s for a test to fail when the message is missing indeed.




---

_Label `testing` added by @MichaReiser on 2025-11-14 11:34_

---

_Label `ty` added by @MichaReiser on 2025-11-14 11:34_

---

_Review requested from @carljm by @MichaReiser on 2025-11-14 11:34_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-14 11:34_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-14 11:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 11:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 11:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-11-14 12:09_

---

_Merged by @MichaReiser on 2025-11-14 12:15_

---

_Closed by @MichaReiser on 2025-11-14 12:15_

---

_Branch deleted on 2025-11-14 12:15_

---
