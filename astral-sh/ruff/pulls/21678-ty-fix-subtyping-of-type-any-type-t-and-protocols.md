```yaml
number: 21678
title: "[ty] Fix subtyping of `type[Any]` / `type[T]` and protocols"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/numpy-workaround
created_at: 2025-11-28T14:19:34Z
updated_at: 2025-11-28T15:56:24Z
url: https://github.com/astral-sh/ruff/pull/21678
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix subtyping of `type[Any]` / `type[T]` and protocols

---

_Pull request opened by @sharkdp on 2025-11-28 14:19_

## Summary

This is a bugfix for subtyping of `type[Any]` / `type[T]` and protocols.

## Test Plan

Regression test that will only be really meaningful once https://github.com/astral-sh/ruff/pull/21553 lands.

---

_Label `ty` added by @sharkdp on 2025-11-28 14:19_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 14:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-28 14:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 500 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/common.py:1702:22: error[invalid-assignment] Object of type `(_SupportsDType[dtype[Any]] & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]]) | (tuple[Any, SupportsIndex | Sequence[SupportsIndex]] & Top[Mapping[Unknown, object]]) | ... omitted 3 union elements` is not assignable to `Mapping[Any, dtype[Any] | None | _SupportsDType[dtype[Any]] | ... omitted 4 union elements]`
+ xarray/core/common.py:1702:22: error[invalid-assignment] Object of type `(type[Any] & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]]) | (tuple[Any, SupportsIndex | Sequence[SupportsIndex]] & Top[Mapping[Unknown, object]]) | ... omitted 4 union elements` is not assignable to `Mapping[Any, dtype[Any] | None | type[Any] | ... omitted 5 union elements]`
- xarray/tests/test_dataarray.py:4157:18: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_dataarray.py:4163:13: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_dataset.py:6905:18: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_dataset.py:6921:18: error[no-matching-overload] No overload of function `full_like` matches arguments
+ xarray/tests/test_plot.py:232:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/tests/test_plot.py:1320:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- xarray/tests/test_variable.py:2837:34: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_variable.py:2844:13: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_variable.py:2863:13: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_variable.py:2883:26: error[no-matching-overload] No overload of function `zeros_like` matches arguments
- xarray/tests/test_variable.py:2883:55: error[no-matching-overload] No overload of function `full_like` matches arguments
- xarray/tests/test_variable.py:2890:26: error[no-matching-overload] No overload of function `ones_like` matches arguments
- xarray/tests/test_variable.py:2890:54: error[no-matching-overload] No overload of function `full_like` matches arguments
- Found 1786 diagnostics
+ Found 1777 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

jax (https://github.com/google/jax)
- jax/_src/cudnn/scaled_matmul_stablehlo.py:689:54: error[invalid-argument-type] Argument to function `scaled_dot_general_transpose_rhs` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `Unknown | list[Unknown]`
+ jax/_src/cudnn/scaled_matmul_stablehlo.py:689:54: error[invalid-argument-type] Argument to function `scaled_dot_general_transpose_rhs` is incorrect: Expected `str | type[Any] | dtype[Any] | SupportsDType`, found `Unknown | list[Unknown]`
- jax/_src/dtypes.py:637:45: error[invalid-assignment] Object of type `(SupportsDType & tuple[object, ...]) | tuple[str | SupportsDType | dtype[Any], ...]` is not assignable to `tuple[str | SupportsDType | dtype[Any], ...]`
+ jax/_src/dtypes.py:637:45: error[invalid-assignment] Object of type `(SupportsDType & tuple[object, ...]) | tuple[str | type[Any] | dtype[Any] | SupportsDType, ...]` is not assignable to `tuple[str | type[Any] | dtype[Any] | SupportsDType, ...]`
- jax/_src/lax/lax.py:5901:54: error[invalid-argument-type] Argument to function `dot_algorithm_attr` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `str | SupportsDType | dtype[Any] | None`
+ jax/_src/lax/lax.py:5901:54: error[invalid-argument-type] Argument to function `dot_algorithm_attr` is incorrect: Expected `str | type[Any] | dtype[Any] | SupportsDType`, found `str | type[Any] | dtype[Any] | SupportsDType | None`
- jax/_src/lax/lax.py:5901:65: error[invalid-argument-type] Argument to function `dot_algorithm_attr` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `str | SupportsDType | dtype[Any] | None`
+ jax/_src/lax/lax.py:5901:65: error[invalid-argument-type] Argument to function `dot_algorithm_attr` is incorrect: Expected `str | type[Any] | dtype[Any] | SupportsDType`, found `str | type[Any] | dtype[Any] | SupportsDType | None`
- jax/_src/lax/linalg.py:2707:19: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/_src/lax/linalg.py:2712:19: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/_src/lax/other.py:224:7: error[invalid-argument-type] Argument to function `conv_general_dilated_patches` is incorrect: Expected `Precision | None`, found `None | Precision | str | SupportsDType | dtype[Any]`
+ jax/_src/lax/other.py:224:7: error[invalid-argument-type] Argument to function `conv_general_dilated_patches` is incorrect: Expected `Precision | None`, found `None | Precision | str | ... omitted 3 union elements`
- jax/_src/numpy/array_methods.py:566:41: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/_src/numpy/lax_numpy.py:5489:37: error[invalid-parameter-default] Default value of type `<class 'float'>` is not assignable to annotated parameter type `str | SupportsDType | dtype[Any]`
- jax/_src/numpy/lax_numpy.py:5637:21: error[invalid-parameter-default] Default value of type `<class 'float'>` is not assignable to annotated parameter type `str | SupportsDType | dtype[Any]`
- jax/_src/numpy/lax_numpy.py:5723:29: error[invalid-parameter-default] Default value of type `<class 'float'>` is not assignable to annotated parameter type `str | SupportsDType | dtype[Any]`
- jax/_src/numpy/lax_numpy.py:8269:60: error[invalid-argument-type] Argument to function `argmax` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'int'>`
- jax/_src/numpy/lax_numpy.py:8330:60: error[invalid-argument-type] Argument to function `argmin` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'int'>`
- jax/_src/numpy/lax_numpy.py:9021:40: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/_src/numpy/lax_numpy.py:9351:81: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `type`
- jax/_src/numpy/reductions.py:2542:35: error[invalid-argument-type] Argument to function `broadcasted_iota` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'int'>`
- jax/_src/numpy/ufuncs.py:1204:40: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/_src/numpy/ufuncs.py:1248:40: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/_src/pallas/mosaic_gpu/lowering.py:1407:13: error[invalid-assignment] Object of type `str | SupportsDType | dtype[Any] | None` is not assignable to `dtype[Any]`
+ jax/_src/pallas/mosaic_gpu/lowering.py:1407:13: error[invalid-assignment] Object of type `str | type[Any] | dtype[Any] | SupportsDType | None` is not assignable to `dtype[Any]`
- jax/_src/pallas/mosaic_gpu/lowering.py:2054:60: error[invalid-argument-type] Argument to function `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/_src/random.py:816:47: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/_src/scipy/linalg.py:2194:28: error[invalid-argument-type] Argument to function `broadcasted_iota` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'float'>`
- jax/_src/tpu/linalg/eigh.py:621:32: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/_src/tpu/linalg/eigh.py:623:48: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/_src/tpu/linalg/eigh.py:627:37: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/_src/tpu/linalg/eigh.py:629:53: error[invalid-argument-type] Argument to function `_tri` is incorrect: Expected `str | SupportsDType | dtype[Any]`, found `<class 'bool'>`
- jax/experimental/jax2tf/tests/flax_models/seq2seq_lstm.py:83:41: error[invalid-argument-type] Argument to function `zeros` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/experimental/pallas/ops/gpu/attention.py:662:68: error[invalid-argument-type] Argument to function `ones` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'bool'>`
- jax/experimental/pallas/ops/gpu/transposed_ragged_dot_mgpu.py:59:21: error[invalid-argument-type] Argument to function `zeros` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/pallas/ops/gpu/transposed_ragged_dot_mgpu.py:60:12: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:582:94: error[invalid-argument-type] Argument to function `zeros` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:1122:53: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:1123:53: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:1124:85: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:1125:85: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:1758:34: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- jax/experimental/sparse/bcoo.py:2238:46: error[invalid-argument-type] Argument to function `array` is incorrect: Expected `str | SupportsDType | dtype[Any] | None`, found `<class 'int'>`
- Found 2733 diagnostics
+ Found 2702 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@sharkdp reviewed on 2025-11-28 14:29_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/libraries/numpy.md`:49 on 2025-11-28 14:29_

These errors prevent us from running into the problem on `main`. But on https://github.com/astral-sh/ruff/pull/21553, these errors will go away, and then we need to properly understand all of this code here, or otherwise we see `np.array` calls failing all over the place.

---

_Marked ready for review by @sharkdp on 2025-11-28 14:29_

---

_Review requested from @carljm by @sharkdp on 2025-11-28 14:29_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-28 14:29_

---

_Review requested from @dcreager by @sharkdp on 2025-11-28 14:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2476 on 2025-11-28 14:33_

but `type[T]` should always be _assignable_ to a protocol-instance if `T`'s upper bound is assignable to the protocol-instance, right? 

```suggestion
            (Type::SubclassOf(self_subclass_ty), Type::ProtocolInstance(_))
                if self_subclass_ty.is_type_var() && relation.is_assignability() =>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2489 on 2025-11-28 14:35_

I think the `type[Dynamic]` assignability case is already handled by the generalised member-lookup approach that `Type::satisfies_protocol()` does in the branch immediately below this one. What we really want to do in this branch is block subtyping for `type[Dynamic]`:

```suggestion
            // `type[Any]` is assignable to arbitrary protocols as it has arbitrary attributes
            // (this is handled by a lower-down branch), but it is only a subtype of
            // a given protocol if `type` is a subtype of that protocol
            (Type::SubclassOf(self_subclass_ty), Type::ProtocolInstance(_))
                if self_subclass_ty.is_dynamic() && !relation.is_assignability() =>
            {
                KnownClass::Type.to_instance(db).has_relation_to_impl(
                    db,
                    target,
                    inferable,
                    relation,
                    relation_visitor,
                    disjointness_visitor,
                ),
            }
```

---

_@AlexWaygood approved on 2025-11-28 14:36_

Some suggestions, but they're untested, so accept them with care 😄

---

_@sharkdp reviewed on 2025-11-28 14:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2476 on 2025-11-28 14:51_

Do you mean: this branch should *return* `ConstraintSet::from(relation.is_assignability())`? Because if we guard it with `is_assignability`, the workaround would not be effective.

---

_@AlexWaygood reviewed on 2025-11-28 14:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2476 on 2025-11-28 14:54_

Ah, right, yeah -- I think I got the polarity the wrong way round?

```suggestion
            (Type::SubclassOf(self_subclass_ty), Type::ProtocolInstance(_))
                if self_subclass_ty.is_type_var() && !relation.is_assignability() =>
```

I don't think the body of the branch should be `ConstraintSet::from(relation.is_assignability())` -- if the relation is `TypeRelation::Assignability`, we only want the assignability check to hold for `type[T] <: P` if the meta-type of `T`'s upper bound is assignable to `P` (we want to fall through to a later branch that should handle that for us)

---

_@sharkdp reviewed on 2025-11-28 14:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2476 on 2025-11-28 14:57_

No, you probably meant `&& !relation.is_assignability()`

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 14:59_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 15:08_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 28 | 4 |
| `no-matching-overload` | 0 | 11 | 0 |
| `invalid-assignment` | 0 | 0 | 3 |
| `invalid-parameter-default` | 0 | 3 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| **Total** | **2** | **42** | **7** |

**[Full report with detailed diff](https://david-numpy-workaround.ecosystem-663.pages.dev/diff)** ([timing results](https://david-numpy-workaround.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood reviewed on 2025-11-28 15:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2494 on 2025-11-28 15:10_

and now that I look at it again, can these not be collapsed into a single branch? `type[T]` should always be a subtype of `P` if `type` is a subtype of `P`, since all `type[T]` branches will be a subtype of `type` no matter what the `T` type variable is solved to. So it's the exact same principle as with `type[Any]`.

```suggestion
            // TODO: The following two branches are a temporary workaround to prevent an important union type
            // in numpy's internals from collapsing to a single type, which causes thousands of downstream
            // false positive diagnostics across the ecosytem. See https://github.com/astral-sh/ty/issues/1666
            // for details.
            //
            // `type[Any]` is assignable to arbitrary protocols as it has arbitrary attributes
            // (this is handled by a lower-down branch), but it is only a subtype of
            // a given protocol if `type` is a subtype of that protocol. Similarly,
            // `type[T]` will always be assignable to any protocol if `type[<upper bound of T>]`
            // is assignable to that protocol (handled lower down), but it is only a
            // subtype of that protocol if `type` is a subtype of that protocol.
            (Type::SubclassOf(self_subclass_ty), Type::ProtocolInstance(_))
                if (self_subclass_ty.is_dynamic() || self_subclass_ty.is_type_var()) && !relation.is_assignability() =>
            {
                KnownClass::Type.to_instance(db).has_relation_to_impl(
                    db,
                    target,
                    inferable,
                    relation,
                    relation_visitor,
                    disjointness_visitor,
                )
            }
```

---

_Renamed from "[ty] Workaround for problem with numpy's `dtype`" to "[ty] Fix subtyping of `type[Any]` / `type[T]` and protocols" by @sharkdp on 2025-11-28 15:24_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-28 15:26_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 15:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:2494 on 2025-11-28 15:26_

Thank you Alex!

---

_@sharkdp reviewed on 2025-11-28 15:26_

---

_Merged by @sharkdp on 2025-11-28 15:56_

---

_Closed by @sharkdp on 2025-11-28 15:56_

---

_Branch deleted on 2025-11-28 15:56_

---
