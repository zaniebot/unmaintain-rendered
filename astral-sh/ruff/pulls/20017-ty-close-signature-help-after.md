```yaml
number: 20017
title: "[ty] Close signature help after `)`"
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
head: micha/offset-1
created_at: 2025-08-21T07:21:44Z
updated_at: 2025-08-22T14:09:24Z
url: https://github.com/astral-sh/ruff/pull/20017
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Close signature help after `)`

---

_Pull request opened by @MichaReiser on 2025-08-21 07:21_

## Summary

The `offset + 1` can panic when the cursor is positioned right before a non-Unicode character. 

This PR uses the same logic as we use for goto definition, etc., to go from `offset` to a range that can be used to find a covering node.

We may want to fine-tune the token prioritization in the future, but I didn't do so as part of this PR (I'd prefer to do this based on user feedback)

I used this as a chance to fix https://github.com/astral-sh/ty/issues/1061 as well

## Test Plan



https://github.com/user-attachments/assets/cf1b0ed5-0770-423d-8082-ed1f3db0f062



---

_Label `server` added by @MichaReiser on 2025-08-21 07:21_

---

_Label `ty` added by @MichaReiser on 2025-08-21 07:21_

---

_Review requested from @carljm by @MichaReiser on 2025-08-21 07:21_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-21 07:21_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-21 07:21_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-21 07:21_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-21 07:22_

---

_Converted to draft by @MichaReiser on 2025-08-21 07:29_

---

_Comment by @github-actions[bot] on 2025-08-21 08:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 08:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] Use tokens for search range in signature help provider" to "[ty] Close signature help after `)`" by @MichaReiser on 2025-08-21 08:12_

---

_Label `bug` added by @MichaReiser on 2025-08-21 08:13_

---

_Marked ready for review by @MichaReiser on 2025-08-21 08:15_

---

_Review requested from @Gankra by @MichaReiser on 2025-08-22 07:54_

---

_Review request for @carljm removed by @MichaReiser on 2025-08-22 07:54_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-08-22 07:54_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-08-22 07:54_

---

_Review request for @dcreager removed by @MichaReiser on 2025-08-22 07:54_

---

_@BurntSushi approved on 2025-08-22 11:43_

Makes sense to me!

---

_@Gankra approved on 2025-08-22 13:39_

nice!

---

_Merged by @MichaReiser on 2025-08-22 14:09_

---

_Closed by @MichaReiser on 2025-08-22 14:09_

---

_Branch deleted on 2025-08-22 14:09_

---
