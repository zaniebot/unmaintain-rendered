```yaml
number: 21701
title: "[ty] remove the `visitor` parameter in the `recursive_type_normalized_impl` method"
type: pull_request
state: merged
author: mtshiba
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: refactor-recursive-type-normalized
created_at: 2025-11-30T04:16:31Z
updated_at: 2025-12-01T07:48:44Z
url: https://github.com/astral-sh/ruff/pull/21701
synced_at: 2026-01-10T16:48:02Z
```

# [ty] remove the `visitor` parameter in the `recursive_type_normalized_impl` method

---

_Pull request opened by @mtshiba on 2025-11-30 04:16_

## Summary

This is a follow-up to #20566.

I revisited the `recursive_type_normalized_impl` method and realized that the `visitor` parameter is no longer necessary (the method doesn't expand type aliases, so there's no risk of infinite recursion), so I removed it.
I also added some more comments to some parts.

## Test Plan

N/A

---

_Comment by @astral-sh-bot[bot] on 2025-11-30 04:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-30 04:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @mtshiba on 2025-11-30 04:44_

---

_Review requested from @carljm by @mtshiba on 2025-11-30 04:44_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-30 04:44_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-30 04:44_

---

_Review requested from @dcreager by @mtshiba on 2025-11-30 04:44_

---

_Label `internal` added by @AlexWaygood on 2025-11-30 10:11_

---

_Label `ty` added by @AlexWaygood on 2025-11-30 10:11_

---

_@MichaReiser approved on 2025-12-01 07:48_

---

_Merged by @MichaReiser on 2025-12-01 07:48_

---

_Closed by @MichaReiser on 2025-12-01 07:48_

---
