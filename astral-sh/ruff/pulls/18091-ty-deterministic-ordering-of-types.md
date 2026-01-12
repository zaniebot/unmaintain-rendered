```yaml
number: 18091
title: "[ty] Deterministic ordering of types"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/stable-ordering
created_at: 2025-05-14T10:06:01Z
updated_at: 2025-05-15T19:01:42Z
url: https://github.com/astral-sh/ruff/pull/18091
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Deterministic ordering of types

---

_@sharkdp_

## Summary

Our type ordering/normalization previously relied on `PartialOrd`/`Ord` instances that were auto-generated for `salsa::interned` structs. These implementations compare by salsa ID, which is not necessarily deterministic (depends on *when* a particular type has been interned, and can therefore depend on file checking order).

Besides not being deterministic, it was also incorrect. When comparing two equal unions `A1 | B1` and `A2 | B2`, it was possible for one union to be ordered incorrectly (e.g. if both `A1` and `B1` were interned nominal instances), leading to incorrect type-relation results.

Writing these `ordering` functions by hand is exhausting and error-prone. This is why I only went so far as to fix the linked bug. Much more work would be involved to implement this for structs like `FunctionType` or `CallableType`. We should probably look into auto-generating these methods somehow?

Related [salsa discussion](https://salsa.zulipchat.com/#narrow/channel/333573-Contributing-to-Salsa/topic/Remove.20.60PartialOrd.60.20and.20.60Ord.60.20derives/with/518011061)

fixes https://github.com/astral-sh/ty/issues/369

## Test Plan

Ran the following command for a while and saw no flaky behavior (on the MRE from the linked ticket):
```bash
while true; do ~/.cargo-target/release/ty check --output-format concise 2>&1 | rg '(Found|checks)'; done
```


---

_Label `ty` added by @sharkdp on 2025-05-14 10:11_

---

_Comment by @github-actions[bot] on 2025-05-14 11:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
- error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:969:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:980:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 645 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-05-14 11:56_

---

_Review requested from @carljm by @sharkdp on 2025-05-14 11:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-14 11:56_

---

_Review requested from @dcreager by @sharkdp on 2025-05-14 11:56_

---

_@MichaReiser reviewed on 2025-05-14 15:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:537 on 2025-05-14 15:24_

Is it guaranteed that `self` and `other` have the same length? 

---

_@MichaReiser reviewed on 2025-05-14 15:26_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:530 on 2025-05-14 15:26_

Nit: Doesn't this depend on a salsa id comparison?

---

_Comment by @MichaReiser on 2025-05-14 15:31_

Hmm, this is indeed painful and also increases the work necessary to normalize types (because it's now necessary to perform a deep comparison). 

> Our type ordering/normalization previously relied on PartialOrd/Ord instances that were auto-generated for salsa::interned structs. These implementations compare by salsa ID, which is not necessarily deterministic (depends on when a particular type has been interned, and can therefore depend on file checking order).

I wonder if this is actually a problem. I'm not sure if it will be when we start garbage collecting interned values but is it today? 



---

_@sharkdp reviewed on 2025-05-14 17:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:537 on 2025-05-14 17:36_

No, thanks for catching that.

---

_@sharkdp reviewed on 2025-05-14 17:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:530 on 2025-05-14 17:39_

Not a nit! Thanks for catching that, I didn't realize `ScopeId` was interned as well.

---

_Comment by @sharkdp on 2025-05-14 17:41_

> I wonder if this is actually a problem. I'm not sure if it will be when we start garbage collecting interned values but is it today?

I don't understand? https://github.com/astral-sh/ty/issues/369#issuecomment-2879256482 definitely feels like an actual problem to me. It's not just non-deterministic, it's also incorrect (sometimes).

---

_Comment by @sharkdp on 2025-05-14 19:00_

> also increases the work necessary to normalize types (because it's now necessary to perform a deep comparison).

The benchmarks seem completely neutral: https://codspeed.io/astral-sh/ruff/branches/david%2Fstable-ordering

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:153 on 2025-05-14 19:43_

I guess this would be tricky to enforce unless salsa avoids adding `PartialOrd`, `Ord` to the interned structs as pointed out by Micha and in the linked salsa discussion.

---

_Comment by @AlexWaygood on 2025-05-14 19:49_

I'm not sure I fully understand yet whether the problem is:
1. That the ordering isn't stable between runs or
2. That some part of ty incorrectly assumes that the ordering will be stable between runs

---

_Comment by @AlexWaygood on 2025-05-14 19:49_

> Besides not being deterministic, it was also incorrect. When comparing two equal unions `A1 | B1` and `A2 | B2`, it was possible for one union to be ordered incorrectly (e.g. if both `A1` and `B1` were interned nominal instances), leading to incorrect type-relation results.

I don't fully understand this point. Could you possibly give an example?

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/generics.rs`:237 on 2025-05-14 19:51_

Do we need to implement `.ordering` for `TypeVarInstance` as it's interned?

---

_@dhruvmanila approved on 2025-05-14 19:52_

---

_Comment by @MichaReiser on 2025-05-14 20:01_

> > I wonder if this is actually a problem. I'm not sure if it will be when we start garbage collecting interned values but is it today?
> 
> 
> 
> I don't understand? https://github.com/astral-sh/ty/issues/369#issuecomment-2879256482 definitely feels like an actual problem to me. It's not just non-deterministic, it's also incorrect (sometimes).

I'm mainly questioning whether we need to override everywhere. We definitely should override it if it otherwise leads to incorrect ordering. I don't think we have to override in cases where it's only about determinism because ids are then a very convenient and fast way to arbitrarily sort items (if the only goal is that they have a fixed ordering but don't require a specific semantic ordering)

It's not quite clear to me what you're fixing in this pr (at least one case seems semantic)

---

_Converted to draft by @sharkdp on 2025-05-14 20:10_

---

_Comment by @carljm on 2025-05-15 02:28_

I think our prior assumption was that ordering of types need only be deterministic within a given run of `ty`, because we should always normalize types as needed to ensure that type ordering differences don't have semantic effects. So if we fix the specific bugs where we fail to normalize or allow type ordering differences to make a semantic difference, that should make it unnecessary to establish a universally consistent type ordering.

---

_Comment by @sharkdp on 2025-05-15 10:29_

> that should make it unnecessary to establish a universally consistent type ordering.

Until we implement persistent caching :-)

---

_Comment by @MichaReiser on 2025-05-15 10:33_

> > that should make it unnecessary to establish a universally consistent type ordering.
> 
> 
> 
> Until we implement persistent caching :-)

Haha, fair. Although salsa might solve this if the rematerialized structs retain their relative order 

---

_Closed by @sharkdp on 2025-05-15 19:01_

---
