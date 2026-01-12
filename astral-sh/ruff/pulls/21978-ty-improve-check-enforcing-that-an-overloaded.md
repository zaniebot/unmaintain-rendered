```yaml
number: 21978
title: "[ty] Improve check enforcing that an overloaded function must have an implementation"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/abcmeta-subclass
created_at: 2025-12-14T17:57:48Z
updated_at: 2025-12-15T08:56:36Z
url: https://github.com/astral-sh/ruff/pull/21978
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Improve check enforcing that an overloaded function must have an implementation

---

_@AlexWaygood_

## Summary

- Treat `if TYPE_CHECKING` blocks the same as stub files (the feature requested in https://github.com/astral-sh/ty/issues/1216)
- We currently only allow `@abstractmethod`-decorated methods to omit the implementation if they're methods in classes that have _exactly_ `ABCMeta` as their metaclass. That seems wrong -- `@abstractmethod` has the same semantics if a class has a subclass of `ABCMeta` as its metaclass. This PR fixes that too. (I'm actually not _totally_ sure we should care what the class's metaclass is at all -- see discussion in https://github.com/astral-sh/ty/issues/1877#issue-3725937441... but the change this PR is making seems less wrong than what we have currently, anyway.)

Fixes https://github.com/astral-sh/ty/issues/1216

## Test Plan

Mdtests and snapshots


---

_Label `ty` added by @AlexWaygood on 2025-12-14 17:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 18:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-14 18:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/mark/structures.py:495:13: error[invalid-overload] Overloads for function `__call__` must be followed by a non-`@overload`-decorated implementation function
- src/_pytest/mark/structures.py:510:13: error[invalid-overload] Overloads for function `__call__` must be followed by a non-`@overload`-decorated implementation function
- src/_pytest/mark/structures.py:542:13: error[invalid-overload] Overloads for function `__call__` must be followed by a non-`@overload`-decorated implementation function
- Found 443 diagnostics
+ Found 440 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-14 18:04_

The primer report LGTM. Those pytest hits are all in an `if TYPE_CHECKING` block: <https://github.com/pytest-dev/pytest/blob/af95858c9293e1fddf05116b1f2a38b62ebd36b5/src/_pytest/mark/structures.py#L488>. That's exactly the feature this PR is trying to enable

---

_Marked ready for review by @AlexWaygood on 2025-12-14 18:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-14 18:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-14 18:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-14 18:06_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-15 07:38_

---

_@sharkdp approved on 2025-12-15 08:05_

Looks good to me — thank you.

---

_@dhruvmanila approved on 2025-12-15 08:20_

---

_Merged by @AlexWaygood on 2025-12-15 08:56_

---

_Closed by @AlexWaygood on 2025-12-15 08:56_

---

_Branch deleted on 2025-12-15 08:56_

---
