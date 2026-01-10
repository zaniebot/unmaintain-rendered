```yaml
number: 21913
title: "[ty] Improve overload call resolution tracing"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/overload-tracing
created_at: 2025-12-11T06:51:35Z
updated_at: 2025-12-11T06:58:47Z
url: https://github.com/astral-sh/ruff/pull/21913
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve overload call resolution tracing

---

_Pull request opened by @dhruvmanila on 2025-12-11 06:51_

This PR improves the overload call resolution tracing messages as:
- Use `trace` level instead of `debug` level
- Add a `trace_span` which contains the call arguments and signature
- Remove the signature from individual tracing messages


---

_Review requested from @carljm by @dhruvmanila on 2025-12-11 06:51_

---

_Label `internal` added by @dhruvmanila on 2025-12-11 06:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-12-11 06:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-11 06:51_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-11 06:51_

---

_Label `ty` added by @dhruvmanila on 2025-12-11 06:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 06:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 06:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5122 diagnostics
+ Found 5123 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @dhruvmanila on 2025-12-11 06:58_

---

_Closed by @dhruvmanila on 2025-12-11 06:58_

---

_Branch deleted on 2025-12-11 06:58_

---
