```yaml
number: 20592
title: "[ty] Fix subtyping for dynamic specializations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-subtyping-for-dynamic-specializations
created_at: 2025-09-26T12:19:15Z
updated_at: 2025-09-26T13:05:05Z
url: https://github.com/astral-sh/ruff/pull/20592
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Fix subtyping for dynamic specializations

---

_Pull request opened by @sharkdp on 2025-09-26 12:19_

## Summary

Fixes a bug observed by @AlexWaygood where `C[Any] <: C[object]` should hold for a class that is covariant in its type parameter (and similar subtyping relations involving dynamic types for other variance configurations).

## Test Plan

New and updated Markdown tests


---

_Review requested from @carljm by @sharkdp on 2025-09-26 12:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-26 12:19_

---

_Review requested from @dcreager by @sharkdp on 2025-09-26 12:19_

---

_Label `ty` added by @sharkdp on 2025-09-26 12:19_

---

_@sharkdp reviewed on 2025-09-26 12:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:305 on 2025-09-26 12:19_

I would argue that these tests were asserting the wrong thing?

---

_@sharkdp reviewed on 2025-09-26 12:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variance.md`:312 on 2025-09-26 12:19_

Same here...

---

_Comment by @github-actions[bot] on 2025-09-26 12:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-26 12:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mkosi (https://github.com/systemd/mkosi)
+ mkosi/config.py:1857:33: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
+ mkosi/config.py:1866:37: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
+ mkosi/config.py:2433:33: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
+ mkosi/config.py:2442:40: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
- Found 110 diagnostics
+ Found 114 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/utils.py:610:17: error[invalid-argument-type] Argument to function `assert_never` is incorrect: Expected `Never`, found `(Unknown & ~Top[Mapping[Unknown, object]] & ~AbstractSet[object]) | (AbstractSet[@Todo] & ~Top[Mapping[Unknown, object]] & ~AbstractSet[object])`
+ pydantic/v1/utils.py:610:17: error[invalid-argument-type] Argument to function `assert_never` is incorrect: Expected `Never`, found `Unknown & ~Top[Mapping[Unknown, object]] & ~AbstractSet[object]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/template.py:829:14: error[unsupported-operator] Operator `<` is not supported for types `slice[Any, Any, Any]` and `int`, in comparing `int | slice[Any, Any, Any]` with `Literal[0]`
- Found 250 diagnostics
+ Found 249 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/engines/utils.py:73:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1584 diagnostics
+ Found 1585 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Sequence[object] | list[Unknown]`
+ torchvision/prototype/utils/_internal.py:34:82: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Sequence[object]`
- torchvision/transforms/functional.py:168:61: warning[possibly-missing-attribute] Attribute `mode` on type `(Image & ~ndarray[object, object]) | (ndarray[@Todo, Unknown] & ~ndarray[object, object])` may be missing
- torchvision/transforms/functional.py:170:8: warning[possibly-missing-attribute] Attribute `mode` on type `(Image & ~ndarray[object, object]) | (ndarray[@Todo, Unknown] & ~ndarray[object, object])` may be missing
- torchvision/transforms/functional.py:172:20: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- torchvision/transforms/functional.py:172:33: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- torchvision/tv_tensors/_tv_tensor.py:82:90: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
- Found 1478 diagnostics
+ Found 1473 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: bytes, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: int | float, json_options: JSONOptions) -> Any) | ((obj: int, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: int | float, json_options: JSONOptions) -> Any) | ((obj: int, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`

apprise (https://github.com/caronc/apprise)
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy) | (dict[Unknown, Unknown] & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy) | (dict[Unknown, Unknown] & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy) | (dict[Unknown, Unknown] & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/datasets/_openml.py:569:9: warning[possibly-missing-attribute] Attribute `update` on type `str | dict[Unknown, Unknown] | list[str] | tuple[int, int] | None | (dict[Unknown, Unknown] & ~AlwaysFalsy)` may be missing
+ sklearn/datasets/_openml.py:569:9: warning[possibly-missing-attribute] Attribute `update` on type `str | dict[Unknown, Unknown] | list[str] | tuple[int, int] | None` may be missing

xarray (https://github.com/pydata/xarray)
- xarray/core/indexing.py:545:38: warning[possibly-missing-attribute] Attribute `dtype` on type `slice[Any, Any, Any] | ndarray[Any, dtype[generic[Any]]]` may be missing
+ xarray/core/indexing.py:550:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

jax (https://github.com/google/jax)
- jax/_src/dtypes.py:984:10: warning[possibly-missing-attribute] Attribute `dtype` on type `(Any & ~None & ~str & ~dtype[object]) | dtype[Any]` may be missing
- jax/_src/numpy/index_tricks.py:87:86: error[not-iterable] Object of type `slice[Any, Any, Any] | tuple[slice[Any, Any, Any], ...]` may not be iterable
- jax/_src/numpy/index_tricks.py:132:86: error[not-iterable] Object of type `slice[Any, Any, Any] | tuple[slice[Any, Any, Any], ...]` may not be iterable
- jax/_src/numpy/lax_numpy.py:4420:51: error[invalid-argument-type] Argument to function `ensure_arraylike_tuple` is incorrect: Expected `Sequence[Any]`, found `(ndarray[@Todo, Unknown] & ~ndarray[object, object] & ~Array) | (Sequence[@Todo] & ~ndarray[object, object] & ~Array)`
- jax/_src/numpy/lax_numpy.py:4582:55: error[invalid-argument-type] Argument to function `ensure_arraylike_tuple` is incorrect: Expected `Sequence[Any]`, found `(ndarray[@Todo, Unknown] & ~ndarray[object, object] & ~Array) | (Sequence[@Todo] & ~ndarray[object, object] & ~Array)`
- jax/_src/numpy/lax_numpy.py:4824:49: error[invalid-argument-type] Argument to function `ensure_arraylike_tuple` is incorrect: Expected `Sequence[Any]`, found `(ndarray[@Todo, Unknown] & ~ndarray[object, object] & ~Array) | (Sequence[@Todo] & ~ndarray[object, object] & ~Array)`
- jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `Mesh | AbstractMesh | None`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `Mesh | AbstractMesh | None`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `AbstractSet[Hashable]`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `AbstractSet[Hashable]`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:143:36: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `Mesh | AbstractMesh | None`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `Mesh | AbstractMesh | None`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `AbstractSet[Hashable]`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `AbstractSet[Hashable]`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | frozenset[Unknown] | bool`
+ jax/_src/shard_map.py:144:24: error[invalid-argument-type] Argument to function `_shard_map` is incorrect: Expected `bool`, found `Mesh | AbstractMesh | None | Any | InferFromArgs | AbstractSet[Hashable] | bool`
- Found 2273 diagnostics
+ Found 2267 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- mypy_django_plugin/django/context.py:375:33: warning[possibly-missing-attribute] Attribute `field` on type `(RelatedField[Any, Any] & ~RelatedField[Never, object]) | (ForeignObjectRel & ~RelatedField[Never, object])` may be missing
- Found 465 diagnostics
+ Found 464 diagnostics

colour (https://github.com/colour-science/colour)
- colour/io/fichet2021.py:863:42: warning[possibly-missing-attribute] Attribute `items` on type `(Any & ~Sequence[object] & ~SpectralDistribution & ~MultiSpectralDistributions & ~ValuesView[object]) | (ValuesView[Unknown] & ~Sequence[object] & ~SpectralDistribution & ~MultiSpectralDistributions & ~ValuesView[object]) | Any` may be missing
- Found 700 diagnostics
+ Found 699 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/commands/navigation.py:321:43: warning[possibly-missing-attribute] Attribute `items` on type `(Sequence[@Todo] & ~Sequence[object]) | (Mapping[str, Sequence[@Todo]] & ~Sequence[object])` may be missing
- Found 196 diagnostics
+ Found 195 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/frame.py:7335:31: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
- static_frame/core/frame.py:7335:61: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
- static_frame/core/frame.py:7337:49: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
+ static_frame/core/frame.py:7346:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any])`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> Index[Any]`
- Found 1878 diagnostics
+ Found 1876 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/legacy/udf/vectorized.py:98:16: error[invalid-return-type] Return type does not match returned value: expected `Series[Any]`, found `ndarray[Unknown, Unknown] & ~Top[list[Unknown]] & ~ndarray[object, object] & ~Top[Series[Any]]`
- Found 3286 diagnostics
+ Found 3285 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/compat/numpy/function.py:110:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[bool, Any]`, found `tuple[bool | (ndarray[@Todo, Unknown] & ~ndarray[object, object]), Unknown]`
+ pandas/compat/numpy/function.py:208:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/core/arrays/datetimes.py:325:20: warning[possibly-missing-attribute] Attribute `_creso` on type `DatetimeTZDtype | (Unknown & ~dtype[object]) | dtype[Any]` may be missing
- pandas/core/indexes/base.py:6766:24: error[unsupported-operator] Operator `+` is unsupported between objects of type `(Unknown & ~ndarray[object, object] & ~slice[object, object, object]) | slice[Any, Any, Any]` and `Literal[1]`
- pandas/core/indexes/base.py:6768:24: error[invalid-return-type] Return type does not match returned value: expected `int`, found `(Unknown & ~ndarray[object, object] & ~slice[object, object, object]) | slice[Any, Any, Any]`
- pandas/core/indexing.py:1767:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~EllipsisType & ~DataFrame & ~slice[object, object, object]) | slice[Any, Any, Any]`
- pandas/core/internals/managers.py:994:42: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `slice[Any, Any, Any] | (@Todo & ~slice[object, object, object])`
- pandas/io/pytables.py:2740:17: error[invalid-argument-type] Argument to function `_unconvert_string_array` is incorrect: Expected `ndarray[@Todo, Unknown]`, found `DatetimeArray | @Todo`
+ pandas/io/pytables.py:2740:17: error[invalid-argument-type] Argument to function `_unconvert_string_array` is incorrect: Expected `ndarray[@Todo, Unknown]`, found `DatetimeArray | @Todo | ndarray[@Todo, @Todo]`
- Found 3325 diagnostics
+ Found 3320 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/solvers/solvers.py:3428:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `set[Basic]` and `tuple[Unknown, ...] | (set[Unknown] & ~AlwaysFalsy) | Unknown | set[Unknown]`
+ sympy/solvers/solvers.py:3428:16: error[unsupported-operator] Operator `&` is unsupported between objects of type `set[Basic]` and `tuple[Unknown, ...] | set[Unknown] | Unknown`

scipy (https://github.com/scipy/scipy)
+ subprojects/highs/src/highspy/highs.py:269:63: error[invalid-argument-type] Argument to function `internal_get_value` is incorrect: Expected `Integral | highs_var | highs_cons | highs_linear_expression | Mapping[Any, Any] | Sequence[Any] | ndarray[Any, dtype[Any]]`, found `object`
- Found 6694 diagnostics
+ Found 6695 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-26 12:22_

---

_Comment by @github-actions[bot] on 2025-09-26 12:27_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 13 | 5 | 14 |
| `possibly-missing-attribute` | 0 | 8 | 1 |
| `unsupported-operator` | 2 | 2 | 4 |
| `index-out-of-bounds` | 0 | 4 | 0 |
| `unused-ignore-comment` | 4 | 0 | 0 |
| `invalid-return-type` | 0 | 3 | 0 |
| `non-subscriptable` | 0 | 2 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `invalid-assignment` | 0 | 0 | 1 |
| `no-matching-overload` | 0 | 1 | 0 |
| **Total** | **19** | **27** | **20** |

**[Full report with detailed diff](https://david-fix-subtyping-for-dyna.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-subtyping-for-dyna.ecosystem-663.pages.dev/timing))


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:854 on 2025-09-26 12:30_

I think you can just inline the condition into the `if` test above now, without the `match`:

```diff
diff --git a/crates/ty_python_semantic/src/types/generics.rs b/crates/ty_python_semantic/src/types/generics.rs
index 2495bdf055..5bdd33eb29 100644
--- a/crates/ty_python_semantic/src/types/generics.rs
+++ b/crates/ty_python_semantic/src/types/generics.rs
@@ -841,16 +841,14 @@ impl<'db> Specialization<'db> {
             .zip(self.types(db))
             .zip(other.types(db))
         {
-            // As an optimization, we can return early if either type is dynamic, unless
-            // we're dealing with a top or bottom materialization.
-            if other_materialization_kind.is_none()
+            // As an optimization, we can return early if `relation` is `TypeRelation::Assignability`
+            // and either type is dynamic, unless we're dealing with a top or bottom materialization.
+            if relation.is_assignability()
+                && other_materialization_kind.is_none()
                 && self_materialization_kind.is_none()
                 && (self_type.is_dynamic() || other_type.is_dynamic())
             {
-                match relation {
-                    TypeRelation::Assignability => continue,
-                    TypeRelation::Subtyping => {}
-                }
+                continue;
             }
```

---

_@AlexWaygood approved on 2025-09-26 12:32_

Thank you!

---

_@AlexWaygood reviewed on 2025-09-26 12:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:854 on 2025-09-26 12:34_

Honestly, there are so many conditions we need to check before we can safely `continue` that I really wonder if this is an optimisation at all. We should short-circuit for `Any` and `Unknown` specializations fairly quickly in the generalised fallthrough logic anyway?

---

_@AlexWaygood reviewed on 2025-09-26 12:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:854 on 2025-09-26 12:34_

But we could experiment separately with removing the optimisation altogether

---

_Comment by @sharkdp on 2025-09-26 12:38_

> ```diff
> + mkosi/config.py:1857:33: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
> + mkosi/config.py:1866:37: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
> + mkosi/config.py:2433:33: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
> + mkosi/config.py:2442:40: error[invalid-argument-type] Argument to function `key_transformer` is incorrect: Expected `str`, found `object`
> ```

This looks like it is caused by https://github.com/astral-sh/ty/issues/1130. The rest of the ecosystem impact looks good, from a first glance.

---

_Comment by @sharkdp on 2025-09-26 13:04_

Benchmarks seem neutral (or even faster)

---

_Merged by @sharkdp on 2025-09-26 13:05_

---

_Closed by @sharkdp on 2025-09-26 13:05_

---

_Branch deleted on 2025-09-26 13:05_

---
