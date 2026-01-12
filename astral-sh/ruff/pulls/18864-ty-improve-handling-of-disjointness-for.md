```yaml
number: 18864
title: "[ty] Improve handling of disjointness for `NominalInstanceType`s and `SubclassOfType`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/disjointness
created_at: 2025-06-22T18:19:38Z
updated_at: 2025-06-24T20:27:38Z
url: https://github.com/astral-sh/ruff/pull/18864
synced_at: 2026-01-12T15:56:26Z
```

# [ty] Improve handling of disjointness for `NominalInstanceType`s and `SubclassOfType`s

---

_@AlexWaygood_

## Summary

This PR makes a number of improvements to our disjointness handling for instance types and `type[]` types:

- The calculation of whether two nominal instance types are disjoint is moved from the somewhat-awkardly-named `NominalInstanceType::is_disjoint_from_nominal_instance_of_class` function to a new `ClassLiteral::could_coexist_in_mro_with()` method. This reflects the fact that the relation between _types_ (the disjointness relation) is derived from a relation between _runtime class objects_ (MRO compatibility). It also allows us to reuse the method for determining disjointness between two `SubclassOfType`s. Prior to now, that wouldn't have been helpful, but with the other improvements in this PR (see below), it _is_ helpful.
- Following https://github.com/astral-sh/ruff/pull/15129, we have understood that classes like this raise `TypeError` at runtime, and have emitted errors if we see such class definitions in first-party code:

  ```py
  class A:
      __slots__ = "a",

  class B:
      __slots__ = "a",

  class C(A, B): ...
  ```

  However, we haven't understood that this also makes the types `A` and `B` disjoint: the intersection `A & B` simplifies to `Never`, since `A` and `B` can never both exist in any single MRO! This PR generalizes and incorporates our understanding of `__slots__` so that we now recognize any class with a non-empty `__slots__` definition as being a "solid base". If there are two solid bases `A` and `B`, and `A` is not a subclass of `B` and `B` is not a subclass of `A`, `A` and `B` will be disjoint. Similarly, if `A` has a subclass `C` and `B` has a subclass `D`, `C` and `D` will be disjoint because they have disjoint solid bases in their respective MROs.
- Two bugs are also fixed in our handling of `__slots__`. One long-standing one is that we used to think that this class definition raised `TypeError`; this PR teaches ty that it doesn't:

  ```py
  class A:
      __slots__ = "a",

  class B(A):
      __slots__ = "b",

  class C(B, A): ...
  ```

  Another one was only introduced recently: see https://github.com/astral-sh/ruff/pull/18864#discussion_r2161608672
- In addition to understanding that classes with non-empty `__slots__` definitions are solid bases, this PR also adds an understanding to ty that certain builtin classes are also solid bases due to the special way that they are implemented in C. This is done via a `KnownClass::is_solid_base` method.
- Lastly, diagnostics for classes that have conflicting solid bases are improved:
  - We now only issue one diagnostic for each class that would raise `TypeError`
  - Subdiagnostics are used to highlight and explain the class bases that create issues

## Test Plan

- Mdtests
- `QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`
- Manual testing of diagnostics on the CLI


---

_Renamed from "alex/disjointness" to "[ty] Improve handling of disjointness for `NominalInstanceType`s and `SubclassOfType`s" by @AlexWaygood on 2025-06-22 18:20_

---

_Label `ty` added by @AlexWaygood on 2025-06-22 18:20_

---

_Comment by @github-actions[bot] on 2025-06-22 18:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- error[incompatible-slots] parso/python/tree.py:169:23: Class base has incompatible `__slots__`
- error[incompatible-slots] parso/python/tree.py:169:34: Class base has incompatible `__slots__`
- Found 79 diagnostics
+ Found 77 diagnostics

operator (https://github.com/canonical/operator)
+ warning[redundant-cast] ops/_private/harness.py:3716:36: Value is already of type `str`
+ warning[redundant-cast] ops/pebble.py:1944:21: Value is already of type `bytes`
- Found 112 diagnostics
+ Found 114 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[invalid-return-type] dulwich/ignore.py:48:12: Return type does not match returned value: expected `str`, found `str | ~bytes`
+ error[invalid-return-type] dulwich/ignore.py:48:12: Return type does not match returned value: expected `str`, found `~bytes`
- error[invalid-argument-type] dulwich/mailmap.py:123:39: Argument to function `parse_identity` is incorrect: Expected `bytes`, found `(bytes & ~tuple[Unknown, ...]) | (tuple[bytes | None, bytes | None] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] dulwich/mailmap.py:123:39: Argument to function `parse_identity` is incorrect: Expected `bytes`, found `bytes | (tuple[bytes | None, bytes | None] & ~tuple[Unknown, ...])`
+ error[unresolved-attribute] dulwich/pack.py:2035:31: Type `int` has no attribute `type_num`
- Found 152 diagnostics
+ Found 153 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[invalid-argument-type] src/check_jsonschema/transforms/azure_pipelines.py:130:26: Argument to function `traverse_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `(dict[Unknown, Unknown] & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])`
+ error[invalid-argument-type] src/check_jsonschema/transforms/azure_pipelines.py:130:26: Argument to function `traverse_dict` is incorrect: Expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | (list[Unknown] & ~list[Unknown])`

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `@Todo(Subscript expressions on intersections) | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
+ error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `@Todo(Subscript expressions on intersections) | int | float | (list[int | float] & ~list[Unknown])`
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:180:14: Attribute `to` on type `int | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:249:12: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:249:27: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:16: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:52: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:251:90: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:254:12: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:254:26: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:16: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:51: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:256:89: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:106:18: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:110:40: Attribute `device` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:110:60: Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:115:43: Attribute `device` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:115:63: Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:120:42: Attribute `pow` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:146:18: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:148:43: Attribute `device` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:148:63: Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:150:22: Attribute `abs` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:276:55: Attribute `exp` on type `@Todo(map_with_boundness: intersections with negative contributions) | int` is possibly unbound
+ error[not-iterable] kornia/geometry/transform/affwarp.py:705:36: Object of type `int` is not iterable
- Found 782 diagnostics
+ Found 805 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-argument-type] pydantic/_internal/_decorators.py:389:49: Argument to function `mro` is incorrect: Expected `type[Any]`, found `(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] pydantic/_internal/_decorators.py:389:49: Argument to function `mro` is incorrect: Expected `type[Any]`, found `type[Any] | (tuple[type[Any], ...] & ~tuple[Unknown, ...])`
- error[invalid-assignment] pydantic/main.py:866:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
+ error[invalid-assignment] pydantic/main.py:866:13: Object of type `tuple[type[Any] | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/main.py:870:67: Argument to function `map_generic_model_arguments` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
+ error[invalid-argument-type] pydantic/main.py:870:67: Argument to function `map_generic_model_arguments` is incorrect: Expected `tuple[Any, ...]`, found `(tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
- error[invalid-assignment] pydantic/v1/generics.py:100:13: Object of type `tuple[(type[Any] & ~tuple[Unknown, ...]) | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
+ error[invalid-assignment] pydantic/v1/generics.py:100:13: Object of type `tuple[type[Any] | (tuple[type[Any], ...] & ~tuple[Unknown, ...])]` is not assignable to `type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/v1/generics.py:106:37: Argument to function `check_parameters_count` is incorrect: Expected `tuple[Any, ...]`, found `(type[Any] & tuple[Unknown, ...]) | (tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
+ error[invalid-argument-type] pydantic/v1/generics.py:106:37: Argument to function `check_parameters_count` is incorrect: Expected `tuple[Any, ...]`, found `(tuple[type[Any], ...] & tuple[Unknown, ...]) | type[Any] | tuple[type[Any], ...]`
- error[invalid-argument-type] pydantic/v1/validators.py:326:26: Argument to bound method `__init__` is incorrect: Expected `bytes | None`, found `(Any & bytes & ~str) | (Any & bytearray & ~str) | UUID`
+ error[invalid-argument-type] pydantic/v1/validators.py:326:26: Argument to bound method `__init__` is incorrect: Expected `bytes | None`, found `(Any & bytes) | (Any & bytearray) | UUID`

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/compiler.py:1133:38: Argument to bound method `ref` is incorrect: Expected `str`, found `(str & ~tuple[Unknown, ...]) | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1133:38: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`
- error[invalid-argument-type] src/jinja2/compiler.py:1136:52: Argument to bound method `ref` is incorrect: Expected `str`, found `(str & ~tuple[Unknown, ...]) | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1136:52: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`
- error[invalid-argument-type] src/jinja2/compiler.py:1149:38: Argument to bound method `ref` is incorrect: Expected `str`, found `(str & ~tuple[Unknown, ...]) | (tuple[str, str] & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/jinja2/compiler.py:1149:38: Argument to bound method `ref` is incorrect: Expected `str`, found `str | (tuple[str, str] & ~tuple[Unknown, ...])`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf]) | (list[Any] & type & ~type[HydraConf]) | (dict[Any, Any] & type & ~type[HydraConf])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:932:24: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf])`
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | (list[Any] & ~type) | (dict[Any, Any] & ~type)`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:941:16: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | list[Any] | dict[Any, Any]`

vision (https://github.com/pytorch/vision)
- error[invalid-assignment] torchvision/models/detection/transform.py:110:13: Object of type `tuple[int & ~list[Unknown] & ~tuple[Unknown, ...]]` is not assignable to `int`
+ error[invalid-assignment] torchvision/models/detection/transform.py:110:13: Object of type `tuple[int]` is not assignable to `int`
- error[invalid-assignment] torchvision/prototype/transforms/_misc.py:56:13: Object of type `dict[Any, (tuple[int, int] & ~dict[Unknown, Unknown]) | (dict[type, tuple[int, int] | None] & ~dict[Unknown, Unknown])]` is not assignable to `tuple[int, int] | dict[type, tuple[int, int] | None]`
+ error[invalid-assignment] torchvision/prototype/transforms/_misc.py:56:13: Object of type `dict[Any, tuple[int, int] | (dict[type, tuple[int, int] | None] & ~dict[Unknown, Unknown])]` is not assignable to `tuple[int, int] | dict[type, tuple[int, int] | None]`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number & ~list[Unknown]) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number & ~list[Unknown]) | (int & tuple[Unknown, ...] & ~list[Unknown]) | (list[int] & tuple[Unknown, ...] & ~list[Unknown]) | (tuple[int, ...] & tuple[Unknown, ...] & ~list[Unknown]) | (int & list[Unknown] & ~list[Unknown]) | (list[int] & list[Unknown] & ~list[Unknown]) | (tuple[int, ...] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
- error[invalid-argument-type] torchvision/transforms/functional.py:1226:45: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `(list[int] & list[Unknown]) | (list[int] & tuple[Unknown, ...]) | list[Unknown]`
- error[invalid-argument-type] torchvision/transforms/functional.py:1226:60: Argument to function `_get_inverse_affine_matrix` is incorrect: Expected `list[int | float]`, found `(list[int] & list[Unknown] & ~tuple[Unknown, ...]) | (list[int] & tuple[Unknown, ...] & ~tuple[Unknown, ...]) | list[Unknown]`
- Found 1512 diagnostics
+ Found 1510 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[invalid-argument-type] src/schemathesis/cli/commands/run/handlers/output.py:1219:32: Argument to function `_print_up_to_three` is incorrect: Expected `list[str] | set[str]`, found `(set[str] & ~dict[Unknown, Unknown]) | (dict[Unknown, Unknown] & ~dict[Unknown, Unknown])`
+ error[invalid-argument-type] src/schemathesis/cli/commands/run/handlers/output.py:1219:32: Argument to function `_print_up_to_three` is incorrect: Expected `list[str] | set[str]`, found `set[str] | (dict[Unknown, Unknown] & ~dict[Unknown, Unknown])`

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[invalid-argument-type] boostedblob/request.py:235:28: Argument to function `dict_to_xml` is incorrect: Expected `Mapping[str, Any]`, found `(dict[str, Any] & ~bytes & ~bytearray) | memoryview[Unknown]`
+ error[invalid-argument-type] boostedblob/request.py:235:28: Argument to function `dict_to_xml` is incorrect: Expected `Mapping[str, Any]`, found `dict[str, Any] | (memoryview[Unknown] & ~memoryview[Unknown])`

mkdocs (https://github.com/mkdocs/mkdocs)
- error[invalid-argument-type] mkdocs/structure/nav.py:223:17: Argument to bound method `__init__` is incorrect: Expected `str`, found `@Todo(map_with_boundness: intersections with negative contributions) | None`
+ error[invalid-argument-type] mkdocs/structure/nav.py:223:17: Argument to bound method `__init__` is incorrect: Expected `str`, found `@Todo(Type::Intersection.call()) | None`

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-argument-type] pwndbg/chain.py:140:21: Argument to function `get` is incorrect: Expected `int | None`, found `(int & ~list[Unknown]) | (list[int] & ~list[Unknown])`
+ error[invalid-argument-type] pwndbg/chain.py:140:21: Argument to function `get` is incorrect: Expected `int | None`, found `int | (list[int] & ~list[Unknown])`

urllib3 (https://github.com/urllib3/urllib3)
- warning[unused-ignore-comment] src/urllib3/contrib/pyopenssl.py:490:58: Unused blanket `type: ignore` directive
- Found 510 diagnostics
+ Found 509 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- warning[possibly-unresolved-reference] com/win32com/test/GenTestScripts.py:72:54: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] com/win32com/test/GenTestScripts.py:78:54: Name `name` used when possibly not defined
- Found 2117 diagnostics
+ Found 2115 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ error[non-subscriptable] schema_salad/jsonld_context.py:132:34: Cannot subscript object of type `None` with no `__getitem__` method
- error[invalid-return-type] schema_salad/sourceline.py:214:12: Return type does not match returned value: expected `int | float | str | CommentedMap | CommentedSeq | None`, found `(int & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (float & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (str & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableMapping[str, Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableSequence[Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | None`
+ error[invalid-return-type] schema_salad/sourceline.py:214:12: Return type does not match returned value: expected `int | float | str | CommentedMap | CommentedSeq | None`, found `(int & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (float & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (str & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableMapping[str, Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | (MutableSequence[Any] & ~CommentedMap & ~CommentedSeq & ~MutableMapping[Unknown, Unknown] & ~MutableSequence[Unknown]) | None`
- Found 162 diagnostics
+ Found 163 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-argument-type] src/bokeh/embed/util.py:199:34: Argument to bound method `__init__` is incorrect: Expected `dict[Model, Unknown]`, found `(list[Model] & ~list[Unknown]) | (dict[Model, Unknown] & ~list[Unknown]) | dict[Unknown, Unknown] | @Todo(dict comprehension type)`
+ error[invalid-argument-type] src/bokeh/embed/util.py:199:34: Argument to bound method `__init__` is incorrect: Expected `dict[Model, Unknown]`, found `(list[Model] & ~list[Unknown]) | dict[Model, Unknown] | dict[Unknown, Unknown] | @Todo(dict comprehension type)`
- error[invalid-argument-type] src/bokeh/util/deprecation.py:70:10: Argument to function `warn` is incorrect: Expected `str`, found `str | (tuple[int, int, int] & ~tuple[Unknown, ...]) | (str & ~tuple[Unknown, ...])`
+ error[invalid-argument-type] src/bokeh/util/deprecation.py:70:10: Argument to function `warn` is incorrect: Expected `str`, found `str | (tuple[int, int, int] & ~tuple[Unknown, ...])`

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-argument-type] lib/tests/streamlit/external/langchain/capturing_callback_handler.py:96:42: Argument to function `load_records_from_file` is incorrect: Expected `str`, found `(list[CallbackRecord] & ~list[Unknown]) | (str & ~list[Unknown])`
+ error[invalid-argument-type] lib/tests/streamlit/external/langchain/capturing_callback_handler.py:96:42: Argument to function `load_records_from_file` is incorrect: Expected `str`, found `(list[CallbackRecord] & ~list[Unknown]) | str`
- error[invalid-return-type] scripts/run_bare_execution_tests.py:59:12: Return type does not match returned value: expected `str`, found `(str & ~list[Unknown]) | (list[str] & ~list[Unknown])`
+ error[invalid-return-type] scripts/run_bare_execution_tests.py:59:12: Return type does not match returned value: expected `str`, found `str | (list[str] & ~list[Unknown])`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ warning[redundant-cast] ddtrace/contrib/internal/subprocess/patch.py:250:26: Value is already of type `list[str]`
- error[invalid-return-type] ddtrace/internal/processor/stats.py:77:12: Return type does not match returned value: expected `tuple[str, str, str, str, int, bool]`, found `tuple[Unknown | str, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Any & ~float & ~AlwaysFalsy) | (int & ~float & ~AlwaysFalsy) | Literal[""], (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], int, bool]`
+ error[invalid-return-type] ddtrace/internal/processor/stats.py:77:12: Return type does not match returned value: expected `tuple[str, str, str, str, int, bool]`, found `tuple[Unknown | str, (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (Any & ~float & ~AlwaysFalsy) | (int & ~AlwaysFalsy) | Literal[""], (str & ~AlwaysFalsy) | Literal[""], (Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[""], int, bool]`
- Found 7038 diagnostics
+ Found 7039 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-return-type] zerver/lib/recipient_parsing.py:10:12: Return type does not match returned value: expected `int`, found `(int & ~list[Unknown]) | (list[int] & ~list[Unknown])`
+ error[invalid-return-type] zerver/lib/recipient_parsing.py:10:12: Return type does not match returned value: expected `int`, found `int | (list[int] & ~list[Unknown])`
- error[invalid-assignment] zerver/lib/test_classes.py:2685:13: Object of type `dict[(str & ~bytes) | None, list[(str & ~bytes) | None]]` is not assignable to `dict[str, list[str]]`
+ error[invalid-assignment] zerver/lib/test_classes.py:2685:13: Object of type `dict[str | None, list[str | None]]` is not assignable to `dict[str, list[str]]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- error[invalid-assignment] sklearn/externals/array_api_extra/_lib/_funcs.py:510:9: Object of type `tuple[(int & ~tuple[Unknown, ...]) | (tuple[int, ...] & ~tuple[Unknown, ...])]` is not assignable to `int | tuple[int, ...]`
+ error[invalid-assignment] sklearn/externals/array_api_extra/_lib/_funcs.py:510:9: Object of type `tuple[int | (tuple[int, ...] & ~tuple[Unknown, ...])]` is not assignable to `int | tuple[int, ...]`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:511:25: Argument to function `len` is incorrect: Expected `Sized`, found `(int & tuple[Unknown, ...]) | (tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:511:25: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(int & tuple[Unknown, ...] & ~tuple[()]) | (tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | (int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
- error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(int & tuple[Unknown, ...] & ~tuple[()]) | (tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | (int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] sklearn/externals/array_api_extra/_lib/_funcs.py:512:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
- error[not-iterable] sklearn/externals/array_api_extra/_lib/_funcs.py:517:40: Object of type `(int & tuple[Unknown, ...]) | (tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable
+ error[not-iterable] sklearn/externals/array_api_extra/_lib/_funcs.py:517:40: Object of type `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable

prefect (https://github.com/PrefectHQ/prefect)
- error[invalid-return-type] src/prefect/cli/cloud/__init__.py:266:12: Return type does not match returned value: expected `str | T`, found `(str & ~tuple[Unknown, ...]) | (tuple[T, str] & ~tuple[Unknown, ...]) | @Todo(Subscript expressions on intersections)`
+ error[invalid-return-type] src/prefect/cli/cloud/__init__.py:266:12: Return type does not match returned value: expected `str | T`, found `str | (tuple[T, str] & ~tuple[Unknown, ...]) | @Todo(Subscript expressions on intersections)`
- error[invalid-argument-type] src/prefect/server/orchestration/core_policy.py:778:17: Argument to function `clamped_poisson_interval` is incorrect: Expected `int | float`, found `@Todo(Subscript expressions on intersections) | (int & ~list[Unknown] & ~AlwaysFalsy) | (float & ~list[Unknown] & ~AlwaysFalsy) | (list[int] & ~list[Unknown] & ~AlwaysFalsy) | (list[int | float] & ~list[Unknown] & ~AlwaysFalsy) | Literal[0]`
+ error[invalid-argument-type] src/prefect/server/orchestration/core_policy.py:778:17: Argument to function `clamped_poisson_interval` is incorrect: Expected `int | float`, found `@Todo(Subscript expressions on intersections) | (int & ~AlwaysFalsy) | (float & ~AlwaysFalsy) | (list[int] & ~list[Unknown] & ~AlwaysFalsy) | (list[int | float] & ~list[Unknown] & ~AlwaysFalsy) | Literal[0]`
- error[invalid-return-type] src/prefect/utilities/templating.py:166:20: Return type does not match returned value: expected `T | type[NotSet]`, found `str & ~int & ~float`
+ error[invalid-return-type] src/prefect/utilities/templating.py:166:20: Return type does not match returned value: expected `T | type[NotSet]`, found `str`
+ error[invalid-assignment] src/prefect/utilities/templating.py:199:25: Object of type `str` is not assignable to `T`
+ error[invalid-assignment] src/prefect/utilities/templating.py:201:21: Object of type `str` is not assignable to `T`
- error[invalid-return-type] src/prefect/utilities/templating.py:203:20: Return type does not match returned value: expected `T | type[NotSet]`, found `(str & ~int & ~float) | @Todo(map_with_boundness: intersections with negative contributions)`
+ error[invalid-return-type] src/prefect/utilities/templating.py:203:20: Return type does not match returned value: expected `T | type[NotSet]`, found `str | T`
- error[invalid-return-type] src/prefect/utilities/templating.py:336:20: Return type does not match returned value: expected `T | dict[str, Any]`, found `str & ~dict[Unknown, Unknown] & ~list[Unknown]`
+ error[invalid-return-type] src/prefect/utilities/templating.py:336:20: Return type does not match returned value: expected `T | dict[str, Any]`, found `str`
- Found 4277 diagnostics
+ Found 4279 diagnostics

scipy (https://github.com/scipy/scipy)
- error[invalid-assignment] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:566:9: Object of type `tuple[(int & ~tuple[Unknown, ...]) | (tuple[int, ...] & ~tuple[Unknown, ...])]` is not assignable to `int | tuple[int, ...]`
+ error[invalid-assignment] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:566:9: Object of type `tuple[int | (tuple[int, ...] & ~tuple[Unknown, ...])]` is not assignable to `int | tuple[int, ...]`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:567:25: Argument to function `len` is incorrect: Expected `Sized`, found `(int & tuple[Unknown, ...]) | (tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:567:25: Argument to function `len` is incorrect: Expected `Sized`, found `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(int & tuple[Unknown, ...] & ~tuple[()]) | (tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | (int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:28: Argument to function `min` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
- error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(int & tuple[Unknown, ...] & ~tuple[()]) | (tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | (int & ~tuple[()]) | (tuple[int, ...] & ~tuple[()])`
+ error[invalid-argument-type] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:568:49: Argument to function `max` is incorrect: Expected `Iterable[Unknown]`, found `(tuple[int, ...] & tuple[Unknown, ...] & ~tuple[()]) | int | (tuple[int, ...] & ~tuple[()])`
- error[not-iterable] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:573:40: Object of type `(int & tuple[Unknown, ...]) | (tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable
+ error[not-iterable] scipy/_lib/array_api_extra/src/array_api_extra/_lib/_funcs.py:573:40: Object of type `(tuple[int, ...] & tuple[Unknown, ...]) | int | tuple[int, ...]` may not be iterable

sympy (https://github.com/sympy/sympy)
- error[incompatible-slots] sympy/codegen/ast.py:872:14: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/ast.py:872:20: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/ast.py:1332:19: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/ast.py:1332:36: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/ast.py:1870:20: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/ast.py:1870:27: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/fnodes.py:673:12: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/fnodes.py:673:19: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/fnodes.py:680:16: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/fnodes.py:680:23: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/matrix_nodes.py:27:19: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/codegen/matrix_nodes.py:27:26: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/add.py:93:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/add.py:93:17: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/expr.py:47:12: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/expr.py:47:19: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/expr.py:4000:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/expr.py:4000:24: Class base has incompatible `__slots__`
+ warning[possibly-unbound-attribute] sympy/core/exprtools.py:345:24: Attribute `p` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/exprtools.py:346:41: Attribute `p` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/exprtools.py:347:37: Attribute `q` on type `(Unknown & Number) | Number | @Todo(Type::Intersection.call())` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/exprtools.py:370:23: Attribute `copy` on type `(Unknown & ~Factors & ~Literal[0] & ~Expr) | None | (Basic & ~Factors & ~Expr)` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/core/exprtools.py:370:23: Attribute `copy` on type `(Unknown & ~Factors & ~Literal[0] & ~Expr) | None | (Basic & ~Expr)` is possibly unbound
- error[incompatible-slots] sympy/core/function.py:382:16: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/function.py:382:29: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/mul.py:91:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/mul.py:91:17: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/relational.py:73:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/relational.py:73:27: Class base has incompatible `__slots__`
+ warning[unused-ignore-comment] sympy/core/symbol.py:227:36: Unused blanket `type: ignore` directive
- error[incompatible-slots] sympy/core/tests/test_power.py:624:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/core/tests/test_power.py:624:27: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:379:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:379:24: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:683:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:683:23: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:801:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/functions/elementary/miscellaneous.py:801:23: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/geometry/entity.py:71:22: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/geometry/entity.py:71:29: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/geometry/entity.py:540:19: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/geometry/entity.py:540:35: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/logic/boolalg.py:573:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/logic/boolalg.py:573:22: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/logic/boolalg.py:746:10: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/logic/boolalg.py:746:21: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/matrices/expressions/matadd.py:19:14: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/matrices/expressions/matadd.py:19:26: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/matrices/expressions/matmul.py:24:14: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/matrices/expressions/matmul.py:24:26: Class base has incompatible `__slots__`
- error[unsupported-operator] sympy/matrices/expressions/slice.py:10:13: Operator `<` is not supported for types `tuple[Unknown, Unknown, Unknown]` and `Literal[0]`, in comparing `(Unknown & ~slice[Any, Any, Any] & ~tuple[Unknown, ...] & ~list[Unknown] & ~Tuple) | (tuple[Unknown, Unknown, Unknown] & ~tuple[Unknown, ...] & ~list[Unknown] & ~Tuple)` with `Literal[0]`
+ error[unsupported-operator] sympy/matrices/expressions/slice.py:10:13: Operator `<` is not supported for types `tuple[Unknown, Unknown, Unknown]` and `Literal[0]`, in comparing `(Unknown & ~slice[Any, Any, Any] & ~tuple[Unknown, ...] & ~list[Unknown] & ~Tuple) | (tuple[Unknown, Unknown, Unknown] & ~tuple[Unknown, ...])` with `Literal[0]`
- error[incompatible-slots] sympy/physics/control/lti.py:366:27: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/physics/control/lti.py:366:34: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/printing/tests/test_fortran.py:656:23: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/printing/tests/test_fortran.py:656:32: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:48:11: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:48:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:1319:13: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:1319:18: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:1496:20: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/sets/sets.py:1496:25: Class base has incompatible `__slots__`
+ warning[unused-ignore-comment] sympy/stats/rv.py:362:55: Unused blanket `type: ignore` directive
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:17:25: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:17:38: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:116:22: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:116:32: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:204:29: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/stats/symbolic_multivariate_probability.py:204:41: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/tensor/tensor.py:2417:15: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/tensor/tensor.py:2417:25: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/tensor/tensor.py:3394:15: Class base has incompatible `__slots__`
- error[incompatible-slots] sympy/tensor/tensor.py:3394:25: Class base has incompatible `__slots__`
- Found 17974 diagnostics
+ Found 17915 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3364 on 2025-06-23 13:13_

This branch looks a little different to how it does on `main`:

https://github.com/astral-sh/ruff/blob/b64307f65ef9fc5029ecd8b22f51adae18eaaed9/crates/ty_python_semantic/src/types/slots.rs#L37-L43

The reason for the change is to fix a regression from https://github.com/astral-sh/ruff/pull/18600 where we started incorrectly reporting that classes like this would raise `TypeError` because the tuples are "not empty":

```py
class Foo:
    __slots__: tuple[str, ...] = ()

class Bar:
    __slots__: tuple[str, ...] = ()

class Baz(Foo, Bar): ...
```

See https://github.com/astral-sh/ruff/pull/18600#issuecomment-2996375418

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:369 on 2025-06-23 13:15_

I renamed the diagnostic from `INCOMPATIBLE_SLOTS` to `INSTANCE_LAYOUT_CONFLICT` because we now detect all of these with the same rule (it doesn't make sense to split them up into different rules IMO):

```py
class A:
    __slots__ = "a",

class B:
    __slots__ = "a",

class C(A, B): ...  # error: [instance-layout-conflict]
class D(A, int): ...  # error: [instance-layout-conflict]
class E(str, int): ...  # error: [instance-layout-conflict]
```

The first is due to "incompatible slots"; the second has something to do with `__slots__` but it's not really accurate to describe the issue as "incompatible slots"; the third has nothing to do with slots -- `str` and `int` don't define `__slots__`, but are considered solid bases anyway for other reasons.

---

_@AlexWaygood reviewed on 2025-06-23 13:16_

---

_Comment by @AlexWaygood on 2025-06-23 13:40_

Primer analysis:
- Lots of `incompatible-slots` false positives going away!
- In general we see fewer long/complicated intersection types, and fewer `@Todo` types, which probably makes for a better user experience overall.
- A lot of new diagnostics on `kornia`. These mostly seem to be because we now understand `int` and `float` as being disjoint types, meaning that `(JustInt | JustFloat) & ~JustFloat` (for example) simplifies to `JustInt` rather than `JustInt & ~JustFloat`. Attribute access on intersections with negative contributions [gives you a `@Todo` type currently](https://play.ty.dev/8187dac0-ee85-4f6e-a0ef-14c6863e6905), so the additional simplifications we're now able to do gives you a stricter type-checking experience. An example of where this kind of narrowing happens in `kornia` is [here](https://github.com/kornia/kornia/blob/a63f4f0f44da0b89b276cbc81ea467ec181d5ca2/kornia/enhance/adjust.py#L37-L49). The type annotation `Union[Tensor, float]` is transformed by us to the type `JustFloat | JustInt | Unknown` due to a combination of the `float`/`int` special case and the fact that we apparently can't resolve the `Tensor` type. Then the first `isinstance()` call causes the type to be intersected with `~JustFloat`, so the type of `Factor` in the `elif isinstance(factor, Tensor):` branch is `JustInt | (Unknown & ~JustFloat)`

In general, I think this is all positive. Though the `kornia` issue makes me wonder if we should intersect with `Unknown` if we see an `isinstance()` call where the second argument is of type `Unknown`, e.g.:

```py
from c_extension import SomethingUnresolved

class Foo: ...

def f(x: Foo | SomethingUnresolved):
    if isinstance(x, SomethingUnresolved):
        reveal_type(x)  # currently `Foo | Unknown`; `(Foo & Unknown) | Unknown` would probably respect the gradual guarantee better?
```

---

_Marked ready for review by @AlexWaygood on 2025-06-23 13:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-23 13:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-23 13:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-23 13:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-23 13:40_

---

_Comment by @AlexWaygood on 2025-06-23 13:43_

Some screenshots of what the `instance-layout-conflict` diagnostics look like on this branch:

![image](https://github.com/user-attachments/assets/f8201079-0ad7-4694-8938-483fa346020e)


---

_Comment by @carljm on 2025-06-23 16:43_

> we should intersect with `Unknown` if we see an `isinstance()` call where the second argument is of type `Unknown`

Yes, I think we should -- it's clearly a violation of the gradual guarantee not to do so.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/elif_else.md`:54 on 2025-06-23 16:49_

I wonder whether it doesn't serve the purpose of this test better to use custom types `A` and `B` and still be able to observe that the effect of narrowing here is the type `A & ~B`, rather than hiding that fact with the use of types that we now understand as disjoint

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:199 on 2025-06-23 16:50_

Similar here -- should we switch to custom types `A` and `B`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/slots.md`:155 on 2025-06-23 16:52_

```suggestion
Certain classes implemented in C extensions are also considered "solid bases" in the same way as
```

---

_Comment by @AlexWaygood on 2025-06-23 17:22_

> > we should intersect with `Unknown` if we see an `isinstance()` call where the second argument is of type `Unknown`
> 
> Yes, I think we should -- it's clearly a violation of the gradual guarantee not to do so.

See #18900

---

_Closed by @AlexWaygood on 2025-06-23 17:26_

---

_Reopened by @AlexWaygood on 2025-06-23 17:26_

---

_Comment by @MichaReiser on 2025-06-23 17:32_

I never heard the term solid base before and searching for it on google also doesn't give any good results. Is there another term we could use or can we explain the term or link to some reference documentation (or hint that it is explained in the rule's documentation)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:368 on 2025-06-23 19:56_

Proposed edit to avoid the term "solid base" in user-facing documentation:

```suggestion
    /// Additionally, this check is not exhaustive: many C extensions (including several in
    /// the standard library) define classes whose instances have an extended memory
    /// layout, and thus can't be multiply-inherited with other such classes.
    /// Since we don't currently represent this fact in stub files, having a full knowledge
    /// of these classes would be impossible; ty only hard-codes a number of known cases.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:369 on 2025-06-23 19:58_

FWIW, ultimately it's all the same issue -- classes whose instances have an extended memory layout beyond just the core object metadata and a dictionary of attributes. `__slots__` is just a Python-facing way to opt into the same internal "slots" system that C-extension classes can opt into directly.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/slots.md`:158 on 2025-06-23 20:00_

```suggestion
Certain classes implemented in C extensions also have an extended instance memory layout, in the
same way as classes that define non-empty `__slots__`. (CPython internally calls all such classes
with a unique instance memory layout "solid bases", and we also borrow this term.) There is currently
no generalized way for ty to detect such a C-extension class, as the information isn't present in type
stubs, but ty special-cases certain builtin classes:

```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/slots.md_-_`__slots__`_-_Built-ins_with_impli…_(8170194eb9ad3094).snap`:57 on 2025-06-23 20:03_

```suggestion
info: Two classes with incompatible instance memory layouts cannot coexist in a class's MRO
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/slots.md_-_`__slots__`_-_Built-ins_with_impli…_(8170194eb9ad3094).snap`:62 on 2025-06-23 20:03_

```suggestion
  |     --- `int` is a C extension type with a distinct instance memory layout
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/slots.md_-_`__slots__`_-_Built-ins_with_impli…_(8170194eb9ad3094).snap`:92 on 2025-06-23 20:04_

```suggestion
   |     - `B` has a distinct instance memory layout because it defines non-empty `__slots__`
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2204 on 2025-06-23 20:07_

```suggestion
/// CPython internally considers a class a "solid base" if it has an atypical instance memory layout,
/// with additional memory "slots" for each instance, besides the default object metadata and an
/// attribute dictionary. A "solid base" can be a class defined in a C extension which defines C-level
/// instance slots, or a Python class that defines non-empty `__slots__`.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:472 on 2025-06-23 20:17_

All tests pass if I replace this line with `solid_base_1 != solid_base_2`.

I think that means we are missing a test for a case like this:

```py
>>> class A:
...     __slots__ = ("a",)
...
>>> class B(A):
...     __slots__ = ("b",)
...
>>> class C(B):
...     pass
...
>>> class D(A):
...     pass
...
>>> class F(C, D):
...     pass
...
>>> F.__mro__
(<class '__main__.F'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.D'>, <class '__main__.A'>, <class 'object'>)
>>> C.__mro__
(<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
>>> D.__mro__
(<class '__main__.D'>, <class '__main__.A'>, <class 'object'>)
```

In this case `C` and `D` have different solid bases (`A` vs `B`), but they can still be multiply inherited because `B` inherits `A`. 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:375 on 2025-06-23 20:22_

```suggestion
    /// Classes that have "dynamic" definitions of `__slots__` (definitions do not consist
    /// of string literals, or tuples of string literals) are not currently recognized by ty as
    /// having a distinct instance layout.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1908 on 2025-06-23 20:24_

May as well match the diagnostic code here, considering its also shorter:

```suggestion
pub(crate) fn report_instance_layout_conflict(
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1925 on 2025-06-23 20:25_

```suggestion
        builder.into_diagnostic("Class will raise `TypeError` at runtime due to incompatible bases");
```

---

_@carljm reviewed on 2025-06-23 20:33_

Nice!

---

_Comment by @carljm on 2025-06-23 20:35_

> I never heard the term solid base before and searching for it on google also doesn't give any good results. Is there another term we could use or can we explain the term or link to some reference documentation (or hint that it is explained in the rule's documentation)

AFAIK the term is only used internally in the CPython code base, not in user-facing docs or error messages. I think we should follow a similar pattern. The term (and concept) are useful in implementing the semantics, and the alternatives are more verbose, so we should go ahead and use the term internally (with a clear definition in a doc comment, which I elaborate on in a review comment above), but avoid the term in user-facing documentation or diagnostics (which I also addressed in review comments above.)

---

_@AlexWaygood reviewed on 2025-06-23 20:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:369 on 2025-06-23 20:54_

Right, but trying to distinguish between "`__slots__`" and "slots" is _asking_ for users to be confused 😆

---

_@AlexWaygood reviewed on 2025-06-23 21:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:368 on 2025-06-23 21:00_

This is better than what I have, thanks! I'll apply a variant of it.

I'm not sure that talking about an "extended memory layout" is _significantly_ more user-friendly than talking about a "solid base", though. I think few Python users think deeply about how their instances are laid out in memory; sort of the point of the language is that you shouldn't have to care most of the time. _I_ certainly don't think about this kind of thing most of the time when I'm writing Python.

The fundamental problem is that this is really a place where CPython leaks implementation details to end users and doesn't really document it properly anywhere. (If only we had a CPython core developer or two around here who could engage upstream and push for improvements there, right?! 😆)

---

_Comment by @AlexWaygood on 2025-06-23 21:10_

> I never heard the term solid base before and searching for it on google also doesn't give any good results. Is there another term we could use or can we explain the term or link to some reference documentation (or hint that it is explained in the rule's documentation)

Yeah, this stuff is really badly documented unfortunately. It's also an important part of Python's semantics (builtin types are common!), though, so I do think it's important that we emulate the semantics... I'll try to improve the docs along the lines of @carljm's suggestions.

I think the best explanation I've found of the semantics might be this stack overflow answer: https://stackoverflow.com/a/48136198/13990016

---

_@carljm reviewed on 2025-06-23 21:53_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:368 on 2025-06-23 21:53_

> The fundamental problem is that this is really a place where CPython leaks implementation details to end users and doesn't really document it properly anywhere.

I agree with this.

> I'm not sure that talking about an "extended memory layout" is _significantly_ more user-friendly than talking about a "solid base", though.

But I don't agree with this. I think talking about memory layouts _is_ significantly better.

For those users who don't understand or care to think about the underlying implementation details, I'll grant that both terms are equally gobbledygook that they'll just have to take our word for. I'm not sure we can do much better than that, for this category of user, given the "fundamental problem" you mentioned.

But there are a significant number of Python users who _do_ have systems programming experience (Micha, for example :) ), or are interested in learning about underlying Python implementation details, and making our diagnostics useful to those users matters as well. And for those users (assuming they haven't worked in the CPython codebase specifically), talking about "solid bases" will still be gobbledygook that doesn't even give useful Google search results, whereas talking about incompatible memory layouts will make sense and give them a good hook for googling and further exploration of the problem.

Also, the diagnostic itself is called `[instance-layout-conflict]`, so we may as well stick to a single way of describing it.


---

_@AlexWaygood reviewed on 2025-06-23 22:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:368 on 2025-06-23 22:09_

Right, I agree that your wording is better here, for all the reasons you mentioned! I just wish we didn't have to talk about "solid bases" _or_ "extended memory layouts" since, if this type checker is successful, my experience from triaging mypy issues suggests we'll have lots of users who are new to programming in general :-)

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-06-24 11:43_

---

_Comment by @codspeed-hq[bot] on 2025-06-24 11:49_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fdisjointness?runnerMode=WallTime)

### Merging #18864 will **degrade performances by 4.79%**

<sub>Comparing <code>alex/disjointness</code> (6aad76f) with <code>main</code> (e44c489)</sub>



### Summary

`❌ 1` regressions  
`✅ 7` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fdisjointness?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[pandas] `` | 32.3 s | 33.9 s | -4.79% |


---

_Comment by @AlexWaygood on 2025-06-24 12:08_

Ecosystem analysis at https://github.com/astral-sh/ruff/actions/runs/15849503790/artifacts/3391568817

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-24 14:55_

---

_@AlexWaygood reviewed on 2025-06-24 15:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:472 on 2025-06-24 15:41_

We actually already had a test like that. The reason why it passed even with your edit is that there was an (unnecessary) check immediately above this that returned `true` if `self.is_subclass_of(other)` or `other.is_subclass_of(self)` returned `true`.

I added your test anyway in https://github.com/astral-sh/ruff/pull/18864/commits/2a8e9854c6749547bed7d4e6b7d4162a7b3e3010 (as it's more complex than mine, and more test coverage is good!) and removed the unnecessary check immediately above this. That also seems to have had the happy side effect of producing a decent speedup when typechecking pydantic...!

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/instance_layout_conf…_-_Tests_for_ty's_`inst…_-_Built-ins_with_impli…_(f5857d64ce69ca1d).snap`:59 on 2025-06-24 19:56_

is this better than the shorter "... if their instances have incompatible memory layouts"?

---

_@carljm approved on 2025-06-24 20:00_

Nice!

There's still an open comment about a rephrasing away from "solid base" in a code comment, but I don't think this is necessary, it's OK if we refer to solid bases in code.

---

_@AlexWaygood reviewed on 2025-06-24 20:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/instance_layout_conf…_-_Tests_for_ty's_`inst…_-_Built-ins_with_impli…_(f5857d64ce69ca1d).snap`:59 on 2025-06-24 20:05_

that's better, thanks!

---

_@carljm approved on 2025-06-24 20:11_

The new diagnostic docs look great!

---

_Comment by @AlexWaygood on 2025-06-24 20:15_

> The new diagnostic docs look great!

Great to hear -- thanks for the excellent review!!

---

_Comment by @AlexWaygood on 2025-06-24 20:25_

For posterity, this is what the final version of these diagnostics looks like:

![image](https://github.com/user-attachments/assets/8db65252-6e1d-4727-b500-2824d58c6da4)


---

_Merged by @AlexWaygood on 2025-06-24 20:27_

---

_Closed by @AlexWaygood on 2025-06-24 20:27_

---

_Branch deleted on 2025-06-24 20:27_

---
