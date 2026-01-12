```yaml
number: 22056
title: "[ty] Make the default database truly statically infallible"
type: pull_request
state: open
author: Gankra
labels:
  - internal
  - ty
assignees: []
base: main
head: gankra/sfinae-ultimate
created_at: 2025-12-18T17:50:06Z
updated_at: 2025-12-19T15:29:57Z
url: https://github.com/astral-sh/ruff/pull/22056
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Make the default database truly statically infallible

---

_@Gankra_

## Summary

I usually hate type wizardry but I decided to have some fun (and also I am legitimately worried that we're going to end up in `Err` whackamole and the previous approach was a huge maintenance hazard).  


---

_Review requested from @carljm by @Gankra on 2025-12-18 17:50_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-18 17:50_

---

_Review requested from @sharkdp by @Gankra on 2025-12-18 17:50_

---

_Review requested from @dcreager by @Gankra on 2025-12-18 17:50_

---

_Label `internal` added by @Gankra on 2025-12-18 17:50_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-18 17:50_

---

_Label `ty` added by @Gankra on 2025-12-18 17:50_

---

_@Gankra reviewed on 2025-12-18 17:51_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/program.rs`:219 on 2025-12-18 17:51_

Here's the heart and soul

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @carljm removed by @carljm on 2025-12-18 17:53_

---

_@Gankra reviewed on 2025-12-18 17:53_

---

_Review comment by @Gankra on `crates/ty_server/src/session.rs`:524 on 2025-12-18 17:53_

Here's the glorious payoff.

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2082 diagnostics
+ Found 2080 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @Gankra on `crates/ty_project/src/glob.rs`:96 on 2025-12-18 17:56_

This is as deep as I was willing to propagate fallback logic. These unwraps/panics really should never ever happen and I'm not worried about them.

---

_@Gankra reviewed on 2025-12-18 17:56_

---

_@Gankra reviewed on 2025-12-18 17:57_

---

_Review comment by @Gankra on `crates/ty_project/src/metadata.rs`:115 on 2025-12-18 17:57_

I was aware of this fallible operation already, I intentionally didn't put handling on it before because the default database doesn't ever reach this branch (but hey, now we Definitely handle it).

---

_@Gankra reviewed on 2025-12-18 17:58_

---

_Review comment by @Gankra on `crates/ty_project/src/metadata/options.rs`:431 on 2025-12-18 17:58_

These are totally new to me and potential failures I never looked into (I expect they never could fail with the default database but hey, now we know they won't).

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 18:01_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @geofft on `crates/ty_python_semantic/src/program.rs`:175 on 2025-12-18 19:10_

```suggestion
/// * [`UseDefaultStrategy`]: The code should apply default values and never fail
```

---

_@geofft reviewed on 2025-12-18 19:27_

I don't think I'm qualified to actually review this but it seems entirely sensible to me!

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:92 on 2025-12-19 10:37_

Would it make sense to instead expose two methods like `ProjectDatabase::new` (falible by default) and `ProjectDatabase::safe`? Ideally, the `unwrap` wouldn't be needed here because we, obviously, want' the safe strategy here

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:431 on 2025-12-19 10:43_

They can't with the default database because `overrides` initializes as `None` but having this enforced is nice

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:170 on 2025-12-19 10:44_

Could we make use of this in many places where we use `.unwrap` today?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:219 on 2025-12-19 10:44_

Is this our own `Try` trait :D

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:317 on 2025-12-19 10:45_

Maybe `ErrStrategy` or `ErrorStrategy`? 



---

_@MichaReiser approved on 2025-12-19 10:47_

This is great. It might make sense for @BurntSushi to also take a look. 

What would be nice is if we could hide this implementation detail from the `ProjectDatabase::new` callers, e.g. by exposing two methods instead of having them pick a strategy (where the `safe` could also do the unwrapping)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-19 14:09_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/program.rs`:170 on 2025-12-19 14:55_

I think you can just use `std::convert::Infallible` here instead. (I don't know if it really matters. Probably not. But seems nicer to use the same type as std.)

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/program.rs`:175 on 2025-12-19 14:59_

What about naming these `FallibleStrategy` and `InfallibleStrategy`?

Although, I do think I like `UseDefaultStrategy` more than `InfallibleStrategy`. I think it's more specific.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/program.rs`:218 on 2025-12-19 15:08_

Great docs. This really helped me understand the approach here.

I might add here that we previously did this at runtime with a simple enum, but that we kept playing whack-a-mole since every call site needed to check the enum. Hence the justification for more sophisticated type wizardry to statically prevent it to force all callers to deal with the possibility of an automatic fallback.

---

_@Gankra reviewed on 2025-12-19 15:12_

---

_Review comment by @Gankra on `crates/ruff_benchmark/benches/ty.rs`:92 on 2025-12-19 15:12_

Hm I think the tests/benches do actually want this unwrap() because we want to immediately slam the breaks if they're misconfigured or if someone has broken a common setup. Unless we're trying to emulate the default database experience?

---

_Review comment by @BurntSushi on `crates/ty_project/src/db.rs`:50 on 2025-12-19 15:13_

Out of curiosity, how come you ended up using an `&Strategy` here instead of just a `Strategy`?

If I were to try and answer my own question, I think it's because this assumes less about `MisconfigurationStrategy`. I think in order to make just `Strategy` work, you'd need to add a `Copy` bound to `MisconfigurationStrategy`?

I guess it doesn't really matter.

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/program.rs`:219 on 2025-12-19 15:13_

Since this is mostly just an internal trait (and probably spiritually sealed at least), I might add a `std::fmt::Debug` constraint here. That way folks can `dbg!` it in random code.

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/ty.rs`:92 on 2025-12-19 15:15_

If we went this route, I'd go with `ProjectDatabase::fallible` and `ProjectDatabase::automatic_defaults` or something like that. I'd avoid `safe` personally, given its confusability with Rust notions of safety.

---

_@BurntSushi approved on 2025-12-19 15:17_

This seems okay to me. I feel like the GAT is mostly bundled up pretty tight. And the docs on the trait helped me understand the approach. :-)

---

_@Gankra reviewed on 2025-12-19 15:29_

---

_Review comment by @Gankra on `crates/ty_project/src/db.rs`:50 on 2025-12-19 15:29_

Yep I just didn't want to add the Copy bound, didn't feel it mattered much.

---
