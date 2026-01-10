```yaml
number: 17021
title: "[red-knot] Don't check non-python files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-no-python-files
created_at: 2025-03-27T19:38:16Z
updated_at: 2025-03-27T20:58:10Z
url: https://github.com/astral-sh/ruff/pull/17021
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Don't check non-python files

---

_Pull request opened by @MichaReiser on 2025-03-27 19:38_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/17018

## Test Plan

I renamed a python file to `knot.toml` and verified that there are no diagnostics. Renaming back the file to `*.py` brings back the diagnostics


---

_Review requested from @carljm by @MichaReiser on 2025-03-27 19:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-27 19:38_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-27 19:38_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-27 19:38_

---

_Label `playground` added by @MichaReiser on 2025-03-27 19:38_

---

_Label `red-knot` added by @MichaReiser on 2025-03-27 19:38_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-27 19:43_

---

_Comment by @github-actions[bot] on 2025-03-27 19:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-03-27 19:45_

---

_Closed by @MichaReiser on 2025-03-27 19:45_

---

_Branch deleted on 2025-03-27 19:45_

---

_Review requested from @Copilot by @AlexWaygood on 2025-03-27 20:57_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-03-27 20:58_

## Pull Request Overview

This PR addresses issue #17018 by ensuring that non-Python files are not checked for diagnostics.  
- In the TypeScript file, a new extension extraction check is added to restrict files to Python-related extensions.  
- In Rust, a new public method (path) is added to convert the file path to a string.

### Reviewed Changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated no comments.

| File                                      | Description                                            |
| ----------------------------------------- | ------------------------------------------------------ |
| playground/knot/src/Editor/Chrome.tsx     | Adds an extension check to filter non-Python files     |
| crates/red_knot_wasm/src/lib.rs            | Adds a new public method for obtaining file path as a string |


<details>
<summary>Comments suppressed due to low confidence (1)</summary>

**playground/knot/src/Editor/Chrome.tsx:223**
* [nitpick] Consider checking whether currentHandle is null before computing the extension. Although safe chaining is used, performing the null check first could improve code clarity.
```
const extension = currentHandle?.path()?.toLowerCase().split(".").pop() ?? "";
```
</details>



---
