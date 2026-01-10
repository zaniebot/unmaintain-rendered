```yaml
number: 20836
title: "[ty] Log files that are slow to type check"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/slow-files
created_at: 2025-10-13T06:57:09Z
updated_at: 2025-10-13T07:15:56Z
url: https://github.com/astral-sh/ruff/pull/20836
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Log files that are slow to type check

---

_Pull request opened by @MichaReiser on 2025-10-13 06:57_

## Summary

Log files that take long to type check to ease debugging cases like https://github.com/astral-sh/ty/issues/1337

This won't help with cases where ty hangs completely and it can also be inaccurate in cases where checking one files requires inferring types for many third party libraries but I think it's better than nothing. 
I picked the 100ms limit arbitrarily. 

## Test Plan

Tested that the logs show for some files that I know that long to type check (in debug).


---

_Label `ty` added by @MichaReiser on 2025-10-13 06:57_

---

_Review requested from @carljm by @MichaReiser on 2025-10-13 06:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-13 06:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-13 06:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-13 06:57_

---

_Label `ty` added by @MichaReiser on 2025-10-13 06:57_

---

_Comment by @github-actions[bot] on 2025-10-13 07:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-13 07:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-10-13 07:14_

Could have made use of this in the past — thanks!

---

_Merged by @MichaReiser on 2025-10-13 07:15_

---

_Closed by @MichaReiser on 2025-10-13 07:15_

---

_Branch deleted on 2025-10-13 07:15_

---
