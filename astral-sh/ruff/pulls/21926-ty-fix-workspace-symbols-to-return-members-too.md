```yaml
number: 21926
title: "[ty] Fix workspace symbols to return members too"
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
head: micha/fix-workspace-symbols
created_at: 2025-12-11T19:01:36Z
updated_at: 2025-12-11T19:22:23Z
url: https://github.com/astral-sh/ruff/pull/21926
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Fix workspace symbols to return members too

---

_@MichaReiser_

## Summary

I think it was an accidental change in https://github.com/astral-sh/ruff/pull/20030 to use `symbols_for_files_global_only` in the workspace symbol request.
The result of it is that you can only find global symbols but not method members.

This PR changes workspace symbols back to use `symbols_for_file`.

Fixes the missing members reported in https://github.com/astral-sh/ty/issues/1856 but not the caching issue

## Test Plan


https://github.com/user-attachments/assets/2a86d850-4c6a-4a70-b483-6955a8090fb1



---

_Label `bug` added by @MichaReiser on 2025-12-11 19:01_

---

_Review requested from @carljm by @MichaReiser on 2025-12-11 19:01_

---

_Label `server` added by @MichaReiser on 2025-12-11 19:01_

---

_Label `ty` added by @MichaReiser on 2025-12-11 19:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-11 19:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-11 19:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-11 19:01_

---

_Label `bug` added by @MichaReiser on 2025-12-11 19:01_

---

_Label `server` added by @MichaReiser on 2025-12-11 19:01_

---

_Label `ty` added by @MichaReiser on 2025-12-11 19:01_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-11 19:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @AlexWaygood on 2025-12-11 19:04_

> ## Test Plan
> 
> [#21385 (files)](https://github.com/astral-sh/ruff/pull/21385/files)

Did you mean to link to a formatting PR by Brent there? 😛

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-11 19:05_

---

_Comment by @MichaReiser on 2025-12-11 19:06_

> Did you mean to link to a formatting PR by Brent there? 😛

Lol no, it pastes my clipboard content when I drag and drop a video into the summary 😕 

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 19:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@BurntSushi approved on 2025-12-11 19:16_

Ah yeah, I think this was unintentional. Oops. Thank you!

---

_Merged by @MichaReiser on 2025-12-11 19:22_

---

_Closed by @MichaReiser on 2025-12-11 19:22_

---

_Branch deleted on 2025-12-11 19:22_

---
