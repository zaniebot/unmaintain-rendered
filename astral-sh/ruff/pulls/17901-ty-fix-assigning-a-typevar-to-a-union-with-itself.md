```yaml
number: 17901
title: "[ty] fix assigning a typevar to a union with itself"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
base: main
head: cjm/tvassign
created_at: 2025-05-06T23:39:07Z
updated_at: 2025-05-07T14:03:39Z
url: https://github.com/astral-sh/ruff/pull/17901
synced_at: 2026-01-10T18:57:03Z
```

# [ty] fix assigning a typevar to a union with itself

---

_Pull request opened by @carljm on 2025-05-06 23:39_

## Summary

Noticed in https://github.com/astral-sh/ruff/pull/17800 that a typevar was not considered assignable to a union containing itself.

A bound typevar `T: int` must be assignable to e.g. `int | None` but must also be assignable to e.g. `T | None`. There's no way to order the bound/constrained-typevar and union arms of `is_subtype_of` and `is_assignable_to` that would achieve both of these; it requires trying the recursive union treatment for one possibility (treat the typevar as itself), and then if that fails, also trying it for the other possibility (treat the typevar as its bound/constraints).

It doesn't work to just put the union case first and let the typevar bound/constraint handling happen one level deeper, because then we fail to consider e.g. `T: (X, Y)` a subtype of `X | Y` -- we would separately try `X | Y <: X` and `X | Y <: Y`, and both fail. (There's already a test for this, fortunately, or I probably would have pushed the simpler fix that breaks it.)

## Test Plan

Added mdtest.


---

_Review requested from @AlexWaygood by @carljm on 2025-05-06 23:39_

---

_Review requested from @sharkdp by @carljm on 2025-05-06 23:39_

---

_Review requested from @dcreager by @carljm on 2025-05-06 23:39_

---

_Label `ty` added by @carljm on 2025-05-06 23:39_

---

_Comment by @github-actions[bot] on 2025-05-06 23:42_

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
- Found 1212 diagnostics
+ Found 1211 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- error[lint:invalid-return-type] arviz/data/inference_data.py:972:20: Return type does not match returned value: Expected `InferenceDataT | None`, found `InferenceDataT`
- error[lint:invalid-return-type] arviz/data/inference_data.py:1052:20: Return type does not match returned value: Expected `InferenceDataT | None`, found `InferenceDataT`
- Found 2605 diagnostics
+ Found 2603 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:121:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:131:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- error[lint:invalid-return-type] lib/streamlit/elements/lib/options_selector_utils.py:150:16: Return type does not match returned value: Expected `E1 | E2`, found `E1`
- Found 3291 diagnostics
+ Found 3288 diagnostics

```
</details>


---

_Comment by @carljm on 2025-05-06 23:45_

Not a huge number of false positives from this, but the ecosystem results all look like fixing false positives.

---

_Comment by @LaBatata101 on 2025-05-07 01:36_

What does this `X | Y <: X` notation means?

---

_Comment by @carljm on 2025-05-07 03:19_

> What does this `X | Y <: X` notation means?

`<:` means "is a subtype of"

---

_Comment by @AlexWaygood on 2025-05-07 10:55_

It would be awesome if we could add a property test for this, but that obviously doesn't need to be done in this PR

---

_@AlexWaygood reviewed on 2025-05-07 10:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1277 on 2025-05-07 10:58_

```suggestion
        // be both a subtype of `int` and a subtype of e.g. `T | None`, so we need to also try
```

---

_Comment by @AlexWaygood on 2025-05-07 11:49_

I proposed a more targeted fix which I like somewhat more in https://github.com/astral-sh/ruff/pull/17910 -- WDYT?

---

_Comment by @carljm on 2025-05-07 14:03_

Closing in favor of https://github.com/astral-sh/ruff/pull/17910

---

_Closed by @carljm on 2025-05-07 14:03_

---
