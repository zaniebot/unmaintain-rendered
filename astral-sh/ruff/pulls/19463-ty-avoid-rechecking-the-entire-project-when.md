```yaml
number: 19463
title: "[ty] Avoid rechecking the entire project when changing the opened files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-reparse-open-change
created_at: 2025-07-21T13:31:33Z
updated_at: 2025-07-21T16:05:05Z
url: https://github.com/astral-sh/ruff/pull/19463
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Avoid rechecking the entire project when changing the opened files

---

_Pull request opened by @MichaReiser on 2025-07-21 13:31_

## Summary

I noticed when playing with the extension that ty always reparses all AST nodes
after I changed the open files in the editor. 

The reason for this is that `check_file_impl` depends on `db.open_files` 
to check if it should evict the AST or not. However, that means that all 
project files will be rechecked after changing the open project files. 


This PR prevents this by moving the AST eviction out of the 
`check_file_impl` function. This has the downside
that it requires an extra `parsed_module` call but that
should be relatively cheap (we make thousends of those when checking a single file).


## Test Plan

I verified that opening an other file doesn't result in re-checking every single project file.


---

_Label `server` added by @MichaReiser on 2025-07-21 13:31_

---

_Label `ty` added by @MichaReiser on 2025-07-21 13:31_

---

_Review requested from @carljm by @MichaReiser on 2025-07-21 13:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-21 13:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-21 13:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-21 13:31_

---

_Review request for @dcreager removed by @MichaReiser on 2025-07-21 13:33_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-07-21 13:33_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-07-21 13:33_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-07-21 13:33_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-07-21 13:33_

---

_Review request for @carljm removed by @MichaReiser on 2025-07-21 13:35_

---

_Comment by @github-actions[bot] on 2025-07-21 13:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@ibraheemdev approved on 2025-07-21 15:27_

---

_Merged by @MichaReiser on 2025-07-21 16:05_

---

_Closed by @MichaReiser on 2025-07-21 16:05_

---

_Branch deleted on 2025-07-21 16:05_

---
