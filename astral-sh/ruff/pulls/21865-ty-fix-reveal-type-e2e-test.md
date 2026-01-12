```yaml
number: 21865
title: "[ty] Fix reveal-type E2E test"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/fix-reveal-type-code-action-test
created_at: 2025-12-09T13:00:43Z
updated_at: 2025-12-09T15:38:57Z
url: https://github.com/astral-sh/ruff/pull/21865
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Fix reveal-type E2E test

---

_@MichaReiser_

## Summary

By migrating it to a `code_actions` test which is much less verbose and easier to review.

## Test Plan

Reviewed test


---

_Label `testing` added by @MichaReiser on 2025-12-09 13:00_

---

_Label `ty` added by @MichaReiser on 2025-12-09 13:00_

---

_Review requested from @carljm by @MichaReiser on 2025-12-09 13:00_

---

_Label `testing` added by @MichaReiser on 2025-12-09 13:00_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-09 13:00_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-09 13:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-09 13:00_

---

_Label `ty` added by @MichaReiser on 2025-12-09 13:00_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-09 13:00_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-09 13:00_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-09 13:00_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-12-09 13:00_

---

_Review requested from @Gankra by @MichaReiser on 2025-12-09 13:00_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 13:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 13:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/testing/internal/test_data.py:148:15: warning[unsupported-base] Unsupported class base with type `<class 'TestItem[Test, Never]'> | <class 'TestItem[Unknown, Unknown]'>`
- tests/testing/internal/test_telemetry.py:392:9: error[invalid-assignment] Implicit shadowing of function `is_benchmark`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5545 diagnostics
+ Found 5544 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2025-12-09 13:08_

---

_Closed by @MichaReiser on 2025-12-09 13:08_

---

_Branch deleted on 2025-12-09 13:08_

---

_@Gankra reviewed on 2025-12-09 15:38_

Thanks!

---
