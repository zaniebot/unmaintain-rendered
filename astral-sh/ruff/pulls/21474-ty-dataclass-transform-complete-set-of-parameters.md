```yaml
number: 21474
title: "[ty] Dataclass transform: complete set of parameters"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclass-transform-remaining-flags
created_at: 2025-11-15T17:33:54Z
updated_at: 2025-11-16T09:46:46Z
url: https://github.com/astral-sh/ruff/pull/21474
synced_at: 2026-01-12T15:57:25Z
```

# [ty] Dataclass transform: complete set of parameters

---

_@sharkdp_

## Summary

We previously only allowed models to overwrite the `{eq,order,kw_only,frozen}_defaults` of the dataclass-transformer, but all other standard-dataclass parameters should be equally supported with the same behavior.

## Test Plan

Added regression tests.

---

_Label `ty` added by @sharkdp on 2025-11-15 17:33_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-15 17:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 17:35_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-15 17:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-15 17:40_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-dataclass-transform-re.ecosystem-663.pages.dev/diff)** ([timing results](https://david-dataclass-transform-re.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @sharkdp on 2025-11-15 19:12_

---

_Review requested from @carljm by @sharkdp on 2025-11-15 19:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-15 19:12_

---

_Review requested from @dcreager by @sharkdp on 2025-11-15 19:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:471 on 2025-11-15 19:58_

isn't `slots=False` the default default? Wouldn't a more interesting test be this?

```suggestion
def fancy_model(*, slots: bool = True) -> Callable[[T], T]:
```

(The same applies to your other examples below!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:625 on 2025-11-15 19:59_

```suggestion
pub(crate) const DATACLASS_FLAGS: &[(&str, DataclassFlags)] = &[
```

---

_@AlexWaygood approved on 2025-11-15 20:00_

Thank you!

---

_@sharkdp reviewed on 2025-11-16 09:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:471 on 2025-11-16 09:41_

The default parameter here has no actual effect on anything. `slots=False` is the default behavior and there is no way to override this for a dataclass-transformer. There is no `slots_default` parameter for the `dataclass_transform` call.

For the parameters that can actually change their defaults (like `kw_only`), we already have tests elsewhere in this file. Here, we just test that we can also modify something like `slots` on an actual *model*/dataclass (not on the transformer / the template).

---

_Merged by @sharkdp on 2025-11-16 09:46_

---

_Closed by @sharkdp on 2025-11-16 09:46_

---

_Branch deleted on 2025-11-16 09:46_

---
