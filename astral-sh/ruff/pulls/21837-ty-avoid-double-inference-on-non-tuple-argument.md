```yaml
number: 21837
title: "[ty] Avoid double-inference on non-tuple argument to `Annotated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-12-08T01:12:46Z
updated_at: 2025-12-08T15:24:08Z
url: https://github.com/astral-sh/ruff/pull/21837
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Avoid double-inference on non-tuple argument to `Annotated`

---

_Pull request opened by @charliermarsh on 2025-12-08 01:12_

## Summary

If you pass a non-tuple to `Annotated`, we end up running inference on it twice. I _think_ the only case here is `Annotated[]`, where we insert a (fake) empty `Name` node in the slice.

Closes https://github.com/astral-sh/ty/issues/1801.


---

_Label `ty` added by @charliermarsh on 2025-12-08 01:12_

---

_Marked ready for review by @charliermarsh on 2025-12-08 01:12_

---

_Review requested from @carljm by @charliermarsh on 2025-12-08 01:12_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-08 01:12_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-08 01:12_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-08 01:12_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 01:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 01:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `bug` added by @AlexWaygood on 2025-12-08 07:35_

---

_@AlexWaygood approved on 2025-12-08 15:05_

---

_Merged by @charliermarsh on 2025-12-08 15:24_

---

_Closed by @charliermarsh on 2025-12-08 15:24_

---

_Branch deleted on 2025-12-08 15:24_

---
