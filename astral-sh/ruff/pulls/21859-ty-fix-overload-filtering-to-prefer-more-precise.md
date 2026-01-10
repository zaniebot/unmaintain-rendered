```yaml
number: 21859
title: "[ty] Fix overload filtering to prefer more \"precise\" match"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/fix-overload-filtering
created_at: 2025-12-09T05:49:19Z
updated_at: 2025-12-09T14:59:37Z
url: https://github.com/astral-sh/ruff/pull/21859
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Fix overload filtering to prefer more "precise" match

---

_Pull request opened by @dhruvmanila on 2025-12-09 05:49_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1809

I took this chance to add some debug level tracing logs for overload call evaluation similar to Doug's implementation in `constraints.rs`.

## Test Plan

- Add new mdtests
- Tested it against `sqlalchemy.select` in pyx which results in the correct overload being matched


---

_Label `bug` added by @dhruvmanila on 2025-12-09 05:49_

---

_Label `ty` added by @dhruvmanila on 2025-12-09 05:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 05:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 05:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scipy-stubs (https://github.com/scipy/scipy-stubs)
+ tests/stats/test_rv_frozen.pyi:35:1: error[type-assertion-failure] Type `int | float` does not match asserted type `Unknown`
+ tests/stats/test_rv_frozen.pyi:40:1: error[type-assertion-failure] Type `int | float` does not match asserted type `Unknown`
- Found 1212 diagnostics
+ Found 1214 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5545 diagnostics
+ Found 5544 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1597 on 2025-12-09 09:33_

The `signature` is important because we want to know which message is this for but this will be too verbose for functions containing multiple overloads. Maybe, it might be better to add some information about the call site instead like which call expression this is for.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2023 on 2025-12-09 09:34_

This is the diff that fixes the issue. I don't really like the change that much, I tried to see if this could be simplified and I have some ideas but might require some more time so I'll punt that on for post-beta stuff to do.

---

_@dhruvmanila reviewed on 2025-12-09 09:34_

---

_Renamed from "[ty] Fix overload filtering" to "[ty] Fix overload filtering to prefer more "precise" match" by @dhruvmanila on 2025-12-09 09:35_

---

_Marked ready for review by @dhruvmanila on 2025-12-09 09:35_

---

_Review requested from @carljm by @dhruvmanila on 2025-12-09 09:35_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-09 09:35_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-09 09:35_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-09 09:35_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/call/bind.rs`:2023 on 2025-12-09 09:47_

Is there a risk of skipping keyword-variadic parameters here? If so, would it be useful to add a variation of the test case where the `def f(*args: Any) -> tuple[Any, ...]: ...` overload also takes keyword arguments (and we would make sure that this overload is still selected if keyword arguments are also passed in)?

---

_@sharkdp approved on 2025-12-09 09:47_

Thank you!!

---

_@dhruvmanila reviewed on 2025-12-09 10:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2023 on 2025-12-09 10:05_

Yeah, this doesn't work well when keyword arguments are involved.

---

_Converted to draft by @dhruvmanila on 2025-12-09 12:01_

---

_Marked ready for review by @dhruvmanila on 2025-12-09 14:53_

---

_@dhruvmanila reviewed on 2025-12-09 14:57_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2023 on 2025-12-09 14:57_

Going to track the follow-up in https://github.com/astral-sh/ty/issues/1825

---

_Merged by @dhruvmanila on 2025-12-09 14:59_

---

_Closed by @dhruvmanila on 2025-12-09 14:59_

---

_Branch deleted on 2025-12-09 14:59_

---
