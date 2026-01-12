```yaml
number: 17910
title: "[ty] fix assigning a typevar to a union with itself"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tvassign
created_at: 2025-05-07T11:43:04Z
updated_at: 2025-05-07T15:50:35Z
url: https://github.com/astral-sh/ruff/pull/17910
synced_at: 2026-01-12T15:56:08Z
```

# [ty] fix assigning a typevar to a union with itself

---

_@AlexWaygood_

## Summary

This is an alternative PR to https://github.com/astral-sh/ruff/pull/17901. It retains the tests from that PR but uses a more targeted fix.

I don't think it's necessary to refactor the `Type::is_subtype_of()` and `Type::is_assignable_to()` methods to the extent that https://github.com/astral-sh/ruff/pull/17901 does. As far as I can see, the problem is specific to unions and intersections containing `TypeVar`s. I don't _think_ there's any situation where a `TypeVar` `T` can be assignable to a type `S` without its upper bound being assignable to `S` unless `S` is a union that directly contains `T`; this is the premise that my alternative implementation is based on.

I also added a couple more tests on top of the ones @carljm added.

## Test Plan

`cargo test -p ty_python_semantic`

Co-authored-by: Carl Meyer <carl@astral.sh>


---

_Review requested from @carljm by @AlexWaygood on 2025-05-07 11:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-07 11:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-07 11:43_

---

_Label `ty` added by @AlexWaygood on 2025-05-07 11:43_

---

_Comment by @github-actions[bot] on 2025-05-07 11:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kopf (https://github.com/nolar/kopf)
- error[lint:invalid-assignment] kopf/_cogs/aiokits/aioenums.py:54:9: Object of type `FlagReasonT | Unknown` is not assignable to `FlagReasonT | None`
- Found 227 diagnostics
+ Found 226 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[lint:invalid-return-type] mkosi/config.py:1092:16: Return type does not match returned value: Expected `SE | None`, found `SE`
- Found 327 diagnostics
+ Found 326 diagnostics

operator (https://github.com/canonical/operator)
- error[lint:invalid-argument-type] ops/pebble.py:1833:67: Argument to this function is incorrect: Expected `AnyStr | None`, found `AnyStr`
- Found 208 diagnostics
+ Found 207 diagnostics

setuptools (https://github.com/pypa/setuptools)
- error[lint:invalid-return-type] setuptools/_distutils/spawn.py:38:16: Return type does not match returned value: Expected `_MappingT | dict[str, str | int] | None`, found `_MappingT | None`
- Found 1209 diagnostics
+ Found 1208 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- error[lint:invalid-return-type] arviz/data/inference_data.py:972:20: Return type does not match returned value: Expected `InferenceDataT | None`, found `InferenceDataT`
- error[lint:invalid-return-type] arviz/data/inference_data.py:1052:20: Return type does not match returned value: Expected `InferenceDataT | None`, found `InferenceDataT`
- Found 2602 diagnostics
+ Found 2600 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:121:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:131:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:150:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- Found 3291 diagnostics
+ Found 3288 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-07 11:48_

The primer results are identical to https://github.com/astral-sh/ruff/pull/17901

---

_Renamed from "[ty] fix assigning a typevar to a union with itself" to "[ty] fix assigning a typevar to a union with itself (alternative version)" by @AlexWaygood on 2025-05-07 11:53_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1003 on 2025-05-07 14:00_

This arm is removable because we (now, as of this PR) handle it in equivalence, which is already checked above.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1335 on 2025-05-07 14:00_

This condition is also removable because it's handled in gradual equivalence, which is checked above.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1575 on 2025-05-07 14:01_

I didn't do this in my PR, but maybe we should add a test for this? Apparently we don't have one.

---

_@carljm approved on 2025-05-07 14:03_

Looks good to me, thank you! I'd vaguely considered this option but didn't love adding special handling for unions and intersections outside of their dedicated arms -- but now that I see it worked through, it's really not bad at all. The DNF normalization really helps us here.

---

_Renamed from "[ty] fix assigning a typevar to a union with itself (alternative version)" to "[ty] fix assigning a typevar to a union with itself" by @carljm on 2025-05-07 14:03_

---

_@AlexWaygood reviewed on 2025-05-07 15:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1003 on 2025-05-07 15:47_

In fact, it was already handled prior to this PR in equivalence! The last branch of the `match` statement handles it in `is_equivalent_to()`; the `if self == other` check before the `match` handles it in `is_gradual_equivalent_to`. The branches that you and I both added in our separate PRs was redundant.

I added tests (as you suggested in https://github.com/astral-sh/ruff/pull/17910/files#r2077710287) that demonstrate this.

---

_Merged by @AlexWaygood on 2025-05-07 15:50_

---

_Closed by @AlexWaygood on 2025-05-07 15:50_

---

_Branch deleted on 2025-05-07 15:50_

---
