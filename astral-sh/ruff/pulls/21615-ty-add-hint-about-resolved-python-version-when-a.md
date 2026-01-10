```yaml
number: 21615
title: "[ty] Add hint about resolved Python version when a user attempts to import a member added on a newer version"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/py-version-import-hint
created_at: 2025-11-24T14:54:19Z
updated_at: 2025-11-24T15:12:03Z
url: https://github.com/astral-sh/ruff/pull/21615
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Add hint about resolved Python version when a user attempts to import a member added on a newer version

---

_Pull request opened by @AlexWaygood on 2025-11-24 14:54_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1620. #20909 added hints if you do something like this and your Python version is set to 3.10 or lower:

```py
import typing

typing.LiteralString
```

And we also have hints if you try to do something like this and your Python version is set too low:

```py
from stdlib_module import new_submodule
```

But we don't currently have any subdiagnostic hint if you do something like _this_ and your Python version is set too low:

```py
from typing import LiteralString
```

This PR adds that hint!

## Test Plan

snapshots


---

_Review requested from @Gankra by @AlexWaygood on 2025-11-24 14:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-24 14:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-24 14:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-24 14:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-24 14:54_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-24 14:54_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/diagnostic.rs`:3634 on 2025-11-24 14:56_

```suggestion
/// The function returns `true` if a hint was added, `false` otherwise.
```

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:58_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2025-11-24 15:01_

Nice!

---

_Label `ty` added by @Gankra on 2025-11-24 15:01_

---

_Merged by @AlexWaygood on 2025-11-24 15:12_

---

_Closed by @AlexWaygood on 2025-11-24 15:12_

---

_Branch deleted on 2025-11-24 15:12_

---
