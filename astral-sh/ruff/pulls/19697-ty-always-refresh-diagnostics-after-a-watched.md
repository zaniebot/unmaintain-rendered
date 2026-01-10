```yaml
number: 19697
title: "[ty] Always refresh diagnostics after a watched files change"
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
head: micha/fix-stale-diagnostics
created_at: 2025-08-01T20:33:41Z
updated_at: 2025-08-04T10:19:19Z
url: https://github.com/astral-sh/ruff/pull/19697
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Always refresh diagnostics after a watched files change

---

_Pull request opened by @MichaReiser on 2025-08-01 20:33_

## Summary

I first thought that this is a bug with workspace diagnostics (and precedence between workspace and pull) but later realized that this is also a problem with regular pull and push diagnostics. 

* User opens file A that imports from file B
* file B changes in a way so that the import in file A is now invalid

The issue is that VS Code doesn't fetch diagnostics on an external (file watcher change). It only does so when an open file changes. The fix in this PR is to always send a diagnostic refresh event after an external change. The only downside of this approach is that the client might request diagnostics when not necessary. But this should, overall, be relatively cheap, given that all the requst should be cached.


Closes https://github.com/astral-sh/ty/issues/925


## Test Plan

**Before**:

No diagnostic, because the client doesn't fetch diagnostics when non-open files changes

https://github.com/user-attachments/assets/6f68c4d7-efeb-43fd-8d47-87d3e2d97630



**Now**

Diagnostic appears.

https://github.com/user-attachments/assets/b55196be-9b72-4c1d-9e3e-e715d4a43a61




---

_Label `bug` added by @MichaReiser on 2025-08-01 20:33_

---

_Label `server` added by @MichaReiser on 2025-08-01 20:33_

---

_Label `ty` added by @MichaReiser on 2025-08-01 20:33_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-08-01 20:35_

---

_Comment by @github-actions[bot] on 2025-08-01 20:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 20:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-08-01 20:37_

---

_Review requested from @carljm by @MichaReiser on 2025-08-01 20:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-08-01 20:37_

---

_Review requested from @sharkdp by @MichaReiser on 2025-08-01 20:37_

---

_Review requested from @dcreager by @MichaReiser on 2025-08-01 20:37_

---

_@dhruvmanila approved on 2025-08-04 06:50_

Agreed. Now that we have diagnostics caching and other optimizations, these requests shouldn't be expensive if not much has changed.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-04 08:39_

---

_Merged by @MichaReiser on 2025-08-04 10:19_

---

_Closed by @MichaReiser on 2025-08-04 10:19_

---

_Branch deleted on 2025-08-04 10:19_

---
