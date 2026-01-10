```yaml
number: 18054
title: "[ty] Infer parameter specializations of explicitly implemented generic protocols"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/specialize-deep-dish
created_at: 2025-05-12T19:51:13Z
updated_at: 2025-05-13T17:13:02Z
url: https://github.com/astral-sh/ruff/pull/18054
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Infer parameter specializations of explicitly implemented generic protocols

---

_Pull request opened by @dcreager on 2025-05-12 19:51_

Follows on from (and depends on) https://github.com/astral-sh/ruff/pull/18021.

This updates our function specialization inference to infer type mappings from parameters that are generic protocols.

For now, this only works when the argument _explicitly_ implements the protocol by listing it as a base class. (We end up using exactly the same logic as for generic classes in #18021.) For this to work with classes that _implicitly_ implement the protocol, we will have to check the types of the protocol members (which we are not currently doing), so that we can infer the specialization of the protocol that the class implements.

---

_Review requested from @carljm by @dcreager on 2025-05-12 19:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-12 19:51_

---

_Review requested from @sharkdp by @dcreager on 2025-05-12 19:51_

---

_Label `ty` added by @dcreager on 2025-05-12 19:51_

---

_Comment by @github-actions[bot] on 2025-05-12 19:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- error[invalid-return-type] src/flake8/processor.py:273:16: Return type does not match returned value: Expected `dict[int, str]`, found `dict[Unknown, LiteralString]`
+ error[invalid-return-type] src/flake8/processor.py:273:16: Return type does not match returned value: Expected `dict[int, str]`, found `dict[int, LiteralString]`

paasta (https://github.com/yelp/paasta)
+ error[invalid-argument-type] paasta_tools/utils.py:1032:17: Argument to bound method `append` is incorrect: Expected `DockerVolume`, found `dict[Unknown, Unknown]`
- Found 978 diagnostics
+ Found 979 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ warning[call-possibly-unbound-method] tests/types/test_array.py:314:12: Method `__getitem__` of type `int | @Todo(list comprehension type)` is possibly unbound
- Found 1200 diagnostics
+ Found 1201 diagnostics

Expression (https://github.com/cognitedata/Expression)
+ error[no-matching-overload] tests/test_map.py:49:10: No overload of function `pipe` matches arguments
- Found 317 diagnostics
+ Found 318 diagnostics

vision (https://github.com/pytorch/vision)
+ error[invalid-argument-type] test/test_transforms_v2.py:944:34: Argument to function `resize` is incorrect: Expected `list[int] | None`, found `int`
+ error[invalid-assignment] torchvision/datasets/_stereo_matching.py:74:13: Object of type `list[str]` is not assignable to `list[None | str]`
+ error[invalid-assignment] torchvision/models/_api.py:235:13: Object of type `set[Unknown | str]` is not assignable to `set[str]`
- Found 2046 diagnostics
+ Found 2049 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-assignment] pwndbg/hexdump.py:97:9: Object of type `list[Unknown]` is not assignable to `bytes`
+ error[invalid-assignment] pwndbg/hexdump.py:97:9: Object of type `list[int]` is not assignable to `bytes`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 650 diagnostics

mypy (https://github.com/python/mypy)
+ error[invalid-assignment] mypy/checker.py:2643:13: Object of type `list[str]` is not assignable to `list[str | None]`
- error[invalid-argument-type] mypy/checker.py:4360:39: Argument to function `erase_type` is incorrect: Expected `Type`, found `None`
- error[invalid-assignment] mypy/checkexpr.py:6387:9: Object of type `None` is not assignable to `Type`
- error[invalid-assignment] mypy/fixup.py:83:21: Object of type `list[Unknown]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
+ error[invalid-assignment] mypy/fixup.py:83:21: Object of type `list[TypeVarLikeType]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
- error[invalid-assignment] mypy/fixup.py:91:21: Object of type `list[Unknown]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
+ error[invalid-assignment] mypy/fixup.py:91:21: Object of type `list[TypeVarLikeType]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
+ error[no-matching-overload] mypy/modulefinder.py:41:34: No overload of function `__new__` matches arguments
+ error[no-matching-overload] mypy/modulefinder.py:43:32: No overload of function `__new__` matches arguments
+ error[no-matching-overload] mypy/modulefinder.py:45:35: No overload of function `__new__` matches arguments
+ error[no-matching-overload] mypy/modulefinder.py:47:36: No overload of function `__new__` matches arguments
- error[invalid-assignment] mypy/semanal.py:2010:9: Object of type `list[Unknown]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
+ error[invalid-assignment] mypy/semanal.py:2010:9: Object of type `list[TypeVarLikeType]` is not assignable to attribute `alias_tvars` on type `TypeAlias | None`
+ warning[possibly-unbound-attribute] mypy/semanal_infer.py:79:20: Attribute `id` on type `Unknown | Type` is possibly unbound
- error[invalid-argument-type] mypy/solve.py:147:58: Argument to function `strongly_connected_components` is incorrect: Expected `dict[Unknown | TypeVarId, list[Unknown | TypeVarId]]`, found `dict[TypeVarId, list[TypeVarId]]`
- error[invalid-argument-type] mypy/solve.py:364:49: Argument to function `is_trivial_bound` is incorrect: Expected `ProperType`, found `None`
- error[invalid-argument-type] mypy/stats.py:295:35: Argument to function `is_imprecise` is incorrect: Expected `Type`, found `None`
+ warning[possibly-unbound-attribute] mypy/test/testmerge.py:195:56: Attribute `names` on type `Unknown | SymbolNode | None` is possibly unbound
+ warning[possibly-unbound-attribute] mypy/typeops.py:512:21: Attribute `type` on type `(Unknown & Type) | Type` is possibly unbound
+ warning[possibly-unresolved-reference] mypy/typeops.py:514:24: Name `arg_type` used when possibly not defined
- error[invalid-return-type] mypyc/analysis/attrdefined.py:298:12: Return type does not match returned value: Expected `set[str]`, found `Unknown | set[str | Unknown]`
+ error[invalid-return-type] mypyc/analysis/attrdefined.py:298:12: Return type does not match returned value: Expected `set[str]`, found `Unknown | set[str | Unknown] | set[str]`
+ error[invalid-assignment] mypyc/ir/func_ir.py:322:5: Object of type `list[Register]` is not assignable to `list[Value]`
+ error[invalid-assignment] mypyc/ir/func_ir.py:350:5: Object of type `list[Register]` is not assignable to `list[Value]`
- Found 3352 diagnostics
+ Found 3358 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ error[unsupported-operator] cwltool/process.py:233:34: Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["File"]` with `str | None`
- Found 324 diagnostics
+ Found 325 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-assignment] src/_pytest/python.py:1432:13: Object of type `dict[Unknown, Literal["direct"]]` is not assignable to `dict[str, Literal["indirect", "direct"]]`
+ error[invalid-assignment] src/_pytest/python.py:1432:13: Object of type `dict[str, Literal["direct"]]` is not assignable to `dict[str, Literal["indirect", "direct"]]`

optuna (https://github.com/optuna/optuna)
+ error[invalid-assignment] optuna/visualization/_parallel_coordinate.py:195:13: Object of type `list[int]` is not assignable to `list[int | float]`
+ error[invalid-argument-type] optuna/visualization/_parallel_coordinate.py:229:17: Argument is incorrect: Expected `list[int | float]`, found `list[int]`
- Found 1185 diagnostics
+ Found 1187 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ error[invalid-return-type] sphinx/domains/python/__init__.py:731:16: Return type does not match returned value: Expected `tuple[list[tuple[str, list[IndexEntry]]], bool]`, found `tuple[list[tuple[Unknown, Unknown]], bool]`
+ error[invalid-assignment] sphinx/environment/adapters/indexentries.py:154:9: Object of type `list[tuple[Unknown, Unknown]]` is not assignable to `list[tuple[str, @Todo(Support for `typing.TypeAlias`)]]`
- error[invalid-argument-type] sphinx/ext/autodoc/preserve_defaults.py:173:72: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] sphinx/ext/autodoc/preserve_defaults.py:179:72: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] sphinx/ext/autodoc/preserve_defaults.py:176:54: Argument to function `get_default_value` is incorrect: Expected `expr`, found `expr | None`
+ error[no-matching-overload] sphinx/ext/autodoc/preserve_defaults.py:178:33: No overload of function `unparse` matches arguments
+ error[invalid-assignment] sphinx/pycode/ast.py:79:9: Object of type `list[expr]` is not assignable to `list[expr | None]`
- Found 709 diagnostics
+ Found 712 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ error[no-matching-overload] src/bokeh/layouts.py:571:39: No overload of function `__new__` matches arguments
+ error[invalid-return-type] src/bokeh/layouts.py:666:16: Return type does not match returned value: Expected `list[L]`, found `list[L | list[L]]`
- error[invalid-return-type] src/bokeh/plotting/_renderer.py:313:12: Return type does not match returned value: Expected `tuple[str, str | None]`, found `tuple[Unknown, ...] | tuple[str, None]`
+ error[invalid-return-type] src/bokeh/plotting/_renderer.py:313:12: Return type does not match returned value: Expected `tuple[str, str | None]`, found `tuple[str, ...] | tuple[str, None]`
- Found 995 diagnostics
+ Found 997 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[unsupported-operator] ddtrace/internal/packages.py:212:9: Operator `|` is unsupported between objects of type `set[Unknown]` and `Unknown | EnvVariable[set[Unknown]]`
+ error[unsupported-operator] ddtrace/internal/packages.py:212:9: Operator `|` is unsupported between objects of type `set[str]` and `Unknown | EnvVariable[set[Unknown]]`
- warning[unused-ignore-comment] ddtrace/internal/symbol_db/symbols.py:577:52: Unused blanket `type: ignore` directive
- Found 8711 diagnostics
+ Found 8710 diagnostics

rotki (https://github.com/rotki/rotki)
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- Found 2104 diagnostics
+ Found 2099 diagnostics

zulip (https://github.com/zulip/zulip)
+ error[invalid-assignment] zerver/actions/create_user.py:154:13: Object of type `list[Unknown | Stream]` is not assignable to `list[Stream]`
+ error[invalid-argument-type] zerver/actions/message_send.py:1592:13: Argument to function `check_any_user_has_permission_by_role` is incorrect: Expected `set[UserProfile]`, found `set[UserProfile | Unknown]`
+ error[invalid-return-type] zerver/lib/users.py:840:12: Return type does not match returned value: Expected `list[int]`, found `list[Unknown | int]`
+ error[invalid-return-type] zerver/lib/users.py:1053:12: Return type does not match returned value: Expected `list[int]`, found `list[Unknown | int]`
+ error[invalid-assignment] zerver/lib/webhooks/git.py:428:5: Object of type `list[tuple[Unknown, Unknown]]` is not assignable to `list[tuple[str, int]]`
- error[invalid-argument-type] zproject/backends.py:174:21: Argument to function `pad_method_dict` is incorrect: Expected `dict[str, bool]`, found `dict[str, bool] | @Todo(instance attribute on class with dynamic base) | dict[Unknown, Literal[True]]`
+ error[invalid-argument-type] zproject/backends.py:174:21: Argument to function `pad_method_dict` is incorrect: Expected `dict[str, bool]`, found `dict[str, bool] | @Todo(instance attribute on class with dynamic base) | dict[str, Literal[True]]`
- Found 4881 diagnostics
+ Found 4886 diagnostics

scipy (https://github.com/scipy/scipy)
+ error[invalid-argument-type] scipy/integrate/tests/test_quadrature.py:118:31: Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, Any, Any]`
+ error[invalid-argument-type] scipy/integrate/tests/test_quadrature.py:139:31: Argument to bound method `insert` is incorrect: Expected `int`, found `slice[Any, Any, Any]`
- error[invalid-assignment] scipy/sparse/linalg/_norm.py:139:9: Not enough values to unpack: Expected 2
+ error[invalid-argument-type] scipy/stats/tests/test_stats.py:1370:18: Argument to bound method `append` is incorrect: Expected `int`, found `int | float`
+ error[invalid-argument-type] scipy/stats/tests/test_stats.py:1371:18: Argument to bound method `append` is incorrect: Expected `int`, found `float`
- Found 7867 diagnostics
+ Found 7870 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ error[invalid-return-type] misc/python/materialize/file_util.py:34:12: Return type does not match returned value: Expected `set[str]`, found `set[Unknown] | set[Unknown | str]`
- Found 3290 diagnostics
+ Found 3291 diagnostics

sympy (https://github.com/sympy/sympy)
+ error[no-matching-overload] sympy/logic/boolalg.py:2850:19: No overload of function `__new__` matches arguments
+ error[not-iterable] sympy/matrices/common.py:3116:55: Object of type `int` is not iterable
+ error[not-iterable] sympy/matrices/common.py:3117:28: Object of type `int` is not iterable
+ error[invalid-argument-type] sympy/matrices/common.py:3118:38: Argument to function `len` is incorrect: Expected `Sized`, found `int`
+ error[invalid-argument-type] sympy/matrices/common.py:3118:53: Argument to function `len` is incorrect: Expected `Sized`, found `int`
+ error[non-subscriptable] sympy/solvers/ode/ode.py:2402:8: Cannot subscript object of type `property` with no `__getitem__` method
+ error[non-subscriptable] sympy/solvers/ode/ode.py:2405:22: Cannot subscript object of type `property` with no `__getitem__` method
+ error[no-matching-overload] sympy/solvers/ode/ode.py:2416:16: No overload of function `subs` matches arguments
+ error[invalid-assignment] sympy/tensor/array/expressions/from_array_to_matrix.py:149:5: Object of type `list[Basic]` is not assignable to `list[Basic | None]`
- Found 17337 diagnostics
+ Found 17346 diagnostics

```
</details>


---

_@AlexWaygood reviewed on 2025-05-12 20:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:20 on 2025-05-12 20:04_

maybe create `ProtocolInstanceType::from_class()` and `ProtocolInstanceType::synthesized()` constructor functions (with comments saying that they should be kept private to the module)? The `_phantom: PhantomData` thing at every callsite feels like it gets a bit repetitive and boilerplate-y

---

_Comment by @AlexWaygood on 2025-05-12 20:05_

Are the primer results here as expected? It looks like there are a bunch of diagnostics being added here

---

_Comment by @dcreager on 2025-05-13 16:19_

Looking at the ecosystem results.

There are several new diagnostics that are instances of https://github.com/astral-sh/ty/issues/336#issuecomment-2874340096, where typevar invariance means that we don't consider e.g. `list[LiteralString]` to be assignable to `list[str]`.

I think this one is a true positive:

```diff
paasta (https://github.com/yelp/paasta)
+ error[invalid-argument-type] paasta_tools/utils.py:1032:17: Argument to bound method `append` is incorrect: Expected `DockerVolume`, found `dict[Unknown, Unknown]`
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:20 on 2025-05-13 16:26_

Done. Did this for `NominalInstanceType` too

---

_@dcreager reviewed on 2025-05-13 16:26_

---

_Closed by @AlexWaygood on 2025-05-13 16:32_

---

_Reopened by @AlexWaygood on 2025-05-13 16:32_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:71 on 2025-05-13 16:37_

```suggestion
    // Keep this field private, so that the only way of constructing `NominalInstanceType` instances
    // is through the `Type::instance` constructor function.
    fn from_class(class: ClassType<'db>) -> Self {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:173 on 2025-05-13 16:37_

```suggestion
    // Keep this field private, so that the only way of constructing `ProtocolInstanceType` instances
    // is through the `Type::instance` constructor function.
    fn from_class(class: ClassType<'db>) -> Self {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:182 on 2025-05-13 16:37_

```suggestion
    // Keep this field private, so that the only way of constructing `ProtocolInstanceType` instances
    // is through the `Type::instance` constructor function.
    fn synthesized(synthesized: SynthesizedProtocolType<'db>) -> Self {
```

---

_@AlexWaygood approved on 2025-05-13 16:38_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:71 on 2025-05-13 16:49_

Going to change this to "Keep this method" etc

---

_@dcreager reviewed on 2025-05-13 16:49_

Surely, **_surely_** I will pay attention to the comment next time if my name is attached to it in the blame! (Narrator: he will not)



---

_Merged by @dcreager on 2025-05-13 17:13_

---

_Closed by @dcreager on 2025-05-13 17:13_

---

_Branch deleted on 2025-05-13 17:13_

---
