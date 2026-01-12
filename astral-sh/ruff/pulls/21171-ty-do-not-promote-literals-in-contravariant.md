```yaml
number: 21171
title: "[ty] Do not promote literals in contravariant positions of generic specializations"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: david/fix-1284
created_at: 2025-10-31T16:23:39Z
updated_at: 2025-10-31T16:48:35Z
url: https://github.com/astral-sh/ruff/pull/21171
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Do not promote literals in contravariant positions of generic specializations

---

_@sharkdp_

## Summary

closes https://github.com/astral-sh/ty/issues/1284

supersedes https://github.com/astral-sh/ruff/pull/20950 by @ibraheemdev 

## Test Plan

New regression test


---

_Label `bug` added by @sharkdp on 2025-10-31 16:23_

---

_Label `ty` added by @sharkdp on 2025-10-31 16:23_

---

_Comment by @github-actions[bot] on 2025-10-31 16:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 16:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/django/user.py:50:53: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Unknown | (EnvVariable[str] & ~AlwaysFalsy) | str`
+ ddtrace/contrib/internal/django/user.py:50:53: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `Unknown | (EnvVariable[Literal[""]] & ~AlwaysFalsy) | str`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-10-31 16:34_

---

_Review requested from @carljm by @sharkdp on 2025-10-31 16:34_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-31 16:34_

---

_Review requested from @dcreager by @sharkdp on 2025-10-31 16:34_

---

_Renamed from "[ty] Do not promote literals in contravariant positions" to "[ty] Do not promote literals in contravariant positions of generic specializations" by @sharkdp on 2025-10-31 16:36_

---

_@ibraheemdev approved on 2025-10-31 16:41_

---

_Merged by @sharkdp on 2025-10-31 16:48_

---

_Closed by @sharkdp on 2025-10-31 16:48_

---

_Branch deleted on 2025-10-31 16:48_

---
