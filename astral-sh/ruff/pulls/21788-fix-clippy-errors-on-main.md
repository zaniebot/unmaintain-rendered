```yaml
number: 21788
title: "Fix clippy errors on `main`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/fix-main
created_at: 2025-12-04T10:45:49Z
updated_at: 2025-12-04T10:50:39Z
url: https://github.com/astral-sh/ruff/pull/21788
synced_at: 2026-01-10T16:48:02Z
```

# Fix clippy errors on `main`

---

_Pull request opened by @dhruvmanila on 2025-12-04 10:45_

https://github.com/astral-sh/ruff/actions/runs/19922070773/job/57112827024#step:5:62

---

_Review requested from @carljm by @dhruvmanila on 2025-12-04 10:45_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-12-04 10:45_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-04 10:45_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-04 10:45_

---

_Label `internal` added by @dhruvmanila on 2025-12-04 10:45_

---

_Renamed from "Fix `main`" to "Fix clippy errors on `main`" by @dhruvmanila on 2025-12-04 10:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 10:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 10:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 38 diagnostics
+ Found 39 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @dhruvmanila on 2025-12-04 10:50_

---

_Closed by @dhruvmanila on 2025-12-04 10:50_

---

_Branch deleted on 2025-12-04 10:50_

---
