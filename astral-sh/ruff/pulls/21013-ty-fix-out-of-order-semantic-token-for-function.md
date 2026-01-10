```yaml
number: 21013
title: "[ty] Fix out-of-order semantic token for function with regular argument after kwargs"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-invalid-semantic-token-order
created_at: 2025-10-21T07:11:01Z
updated_at: 2025-10-21T08:51:11Z
url: https://github.com/astral-sh/ruff/pull/21013
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix out-of-order semantic token for function with regular argument after kwargs

---

_Pull request opened by @MichaReiser on 2025-10-21 07:11_

## Summary

Fixes a bug where the semantic token provider returned out-of-order tokens for
function definitions with a regular parameter coming after a kwargs parameter:


```py
def foo(self, **key, value):
    return
```

The LSP specification isn't explicit whether out-of-order tokens are allowed, but
the way it reads suggests that providers are expected to return tokens in-order. 

The root cause for this issue is that `Parameters::iter()` only guarantees to
return the parameters in source order if there are no syntax errors because of how
parameters are represented internally (different vectors for the different parameter kinds). 

The ideal fix would be to change our `Parameters` definition but that's a bit more work,
which is why I decided to sort the parameters in place instead.

Fixes https://github.com/astral-sh/ty/issues/1406


## Test Plan

Added test


---

_Review requested from @carljm by @MichaReiser on 2025-10-21 07:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-21 07:11_

---

_Label `bug` added by @MichaReiser on 2025-10-21 07:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-21 07:11_

---

_Label `server` added by @MichaReiser on 2025-10-21 07:11_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-21 07:11_

---

_Label `ty` added by @MichaReiser on 2025-10-21 07:11_

---

_Comment by @github-actions[bot] on 2025-10-21 07:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 07:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-21 07:41_

Thank you!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-21 08:03_

---

_Merged by @MichaReiser on 2025-10-21 08:51_

---

_Closed by @MichaReiser on 2025-10-21 08:51_

---

_Branch deleted on 2025-10-21 08:51_

---
