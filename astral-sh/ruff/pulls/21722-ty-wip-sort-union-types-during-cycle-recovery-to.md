```yaml
number: 21722
title: "[ty] WIP: sort union types during cycle recovery to stabilize output"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
assignees: []
draft: true
base: main
head: sort-recursive-union
created_at: 2025-12-01T10:59:03Z
updated_at: 2025-12-17T17:03:13Z
url: https://github.com/astral-sh/ruff/pull/21722
synced_at: 2026-01-10T16:42:11Z
```

# [ty] WIP: sort union types during cycle recovery to stabilize output

---

_Pull request opened by @mtshiba on 2025-12-01 10:59_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1670

This PR addresses the issue of non-deterministic execution order of salsa queries causing the order of elements in union types in the output to fluctuate randomly.

We already have a comparison function for sorting union types, `union_or_intersection_elements_ordering`, but this probably won't solve the problem, since it compares based on salsa IDs rather than the contents of the interned structs.

Instead, we use the `structural_type_ordering` function, which performs comparisons by deep traversing the type's contents.

## Test Plan



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 11:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 11:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:441:51: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:441:51: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:444:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:444:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:449:29: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:449:29: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `IndentationNode | None | Unknown`
- parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `IndentationNode | None | Unknown`

spack (https://github.com/spack/spack)
- lib/spack/spack/cmd/create.py:1059:45: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[True]`
+ lib/spack/spack/cmd/create.py:1059:45: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `Literal[True] | (str & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
- lib/spack/spack/environment/environment.py:2457:26: error[invalid-assignment] Object of type `Unknown | None | str | bool` is not assignable to `str`
+ lib/spack/spack/environment/environment.py:2457:26: error[invalid-assignment] Object of type `None | bool | str | Unknown` is not assignable to `str`
- lib/spack/spack/externals.py:88:9: error[invalid-assignment] Object of type `Microarchitecture` is not assignable to attribute `target` on type `(Unknown & ~AlwaysTruthy) | None | (ArchSpec & ~AlwaysTruthy)`
+ lib/spack/spack/externals.py:88:9: error[invalid-assignment] Object of type `Microarchitecture` is not assignable to attribute `target` on type `(ArchSpec & ~AlwaysTruthy) | None | (Unknown & ~AlwaysTruthy)`
- lib/spack/spack/installer.py:580:44: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `Unknown | None | str | bool`
+ lib/spack/spack/installer.py:580:44: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `None | bool | str | Unknown`
- lib/spack/spack/installer.py:583:24: error[invalid-argument-type] Argument to function `create_or_construct` is incorrect: Expected `str | None`, found `Unknown | None | str | bool`
+ lib/spack/spack/installer.py:583:24: error[invalid-argument-type] Argument to function `create_or_construct` is incorrect: Expected `str | None`, found `None | bool | str | Unknown`
- lib/spack/spack/solver/asp.py:2650:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~None) | ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:2650:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `ConcreteVersion | (Unknown & ~None)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/solver/asp.py:3020:17: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `(Unknown & ~None) | str | bool` on object of type `dict[str, str]`
+ lib/spack/spack/solver/asp.py:3020:17: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `bool | str | (Unknown & ~None)` on object of type `dict[str, str]`
- lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~AlwaysFalsy) | (ConcreteVersion & ~AlwaysFalsy)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(ConcreteVersion & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/spec.py:3109:21: warning[possibly-missing-attribute] Attribute `intersects` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:3109:21: warning[possibly-missing-attribute] Attribute `intersects` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/spec.py:3130:24: warning[possibly-missing-attribute] Attribute `constrain` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:3130:24: warning[possibly-missing-attribute] Attribute `constrain` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/spec.py:3654:17: error[invalid-assignment] Object of type `Any & ~AlwaysFalsy` is not assignable to attribute `_patches_in_order_of_appearance` on type `Unknown | VariantValue`
+ lib/spack/spack/spec.py:3654:17: error[invalid-assignment] Object of type `Any & ~AlwaysFalsy` is not assignable to attribute `_patches_in_order_of_appearance` on type `VariantValue | Unknown`
- lib/spack/spack/spec.py:4593:16: warning[possibly-missing-attribute] Attribute `platform` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:4593:16: warning[possibly-missing-attribute] Attribute `platform` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/spec.py:4597:16: warning[possibly-missing-attribute] Attribute `os` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:4597:16: warning[possibly-missing-attribute] Attribute `os` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/spec.py:4601:16: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:4601:16: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/spec.py:5290:17: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_patches_in_order_of_appearance` on type `Unknown | VariantValue`
+ lib/spack/spack/spec.py:5290:17: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `_patches_in_order_of_appearance` on type `VariantValue | Unknown`
- lib/spack/spack/test/architecture.py:64:12: warning[possibly-missing-attribute] Attribute `os` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/architecture.py:64:12: warning[possibly-missing-attribute] Attribute `os` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/architecture.py:65:12: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/architecture.py:65:12: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/concretization/core.py:604:24: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/concretization/core.py:604:24: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/concretization/core.py:604:49: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/concretization/core.py:604:49: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/concretization/core.py:1111:20: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/concretization/core.py:1111:20: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/concretization/core.py:1562:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/concretization/core.py:1562:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `tuple[bool | str, ...] | bool | str | Unknown`
- lib/spack/spack/test/concretization/core.py:1575:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/concretization/core.py:1575:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `tuple[bool | str, ...] | bool | str | Unknown`
- lib/spack/spack/test/concretization/core.py:2159:64: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/concretization/core.py:2159:64: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/test/patch.py:173:9: warning[possibly-missing-attribute] Attribute `_patches_in_order_of_appearance` may be missing on object of type `Unknown | VariantValue`
+ lib/spack/spack/test/patch.py:173:9: warning[possibly-missing-attribute] Attribute `_patches_in_order_of_appearance` may be missing on object of type `VariantValue | Unknown`
- lib/spack/spack/test/patch.py:183:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["a69b288d7393261e613c276c6d38a01461028291f6e381623acc58139d01f54d"]` and `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/patch.py:183:12: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["a69b288d7393261e613c276c6d38a01461028291f6e381623acc58139d01f54d"]` and `tuple[bool | str, ...] | bool | str | Unknown`
- lib/spack/spack/test/patch.py:186:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["a69b288d7393261e613c276c6d38a01461028291f6e381623acc58139d01f54d"]` and `Unknown | tuple[bool | str, ...] | bool | str`
+ lib/spack/spack/test/patch.py:186:12: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal["a69b288d7393261e613c276c6d38a01461028291f6e381623acc58139d01f54d"]` and `tuple[bool | str, ...] | bool | str | Unknown`
- lib/spack/spack/test/spec_semantics.py:1074:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["test"]` and `Unknown | None | ArchSpec`
+ lib/spack/spack/test/spec_semantics.py:1074:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["test"]` and `ArchSpec | None | Unknown`
- lib/spack/spack/test/spec_semantics.py:1075:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["debian"]` and `Unknown | None | ArchSpec`
+ lib/spack/spack/test/spec_semantics.py:1075:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["debian"]` and `ArchSpec | None | Unknown`
- lib/spack/spack/test/spec_semantics.py:1076:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["x86_64"]` and `Unknown | None | ArchSpec`
+ lib/spack/spack/test/spec_semantics.py:1076:16: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["x86_64"]` and `ArchSpec | None | Unknown`
- lib/spack/spack/test/spec_syntax.py:1765:12: warning[possibly-missing-attribute] Attribute `platform` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/test/spec_syntax.py:1765:12: warning[possibly-missing-attribute] Attribute `platform` may be missing on object of type `ArchSpec | None | Unknown`
- lib/spack/spack/vendor/pyrsistent/_plist.py:22:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `rest` on type `(Unknown & ~AlwaysFalsy) | (_EmptyPList & ~AlwaysFalsy)`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:22:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `rest` on type `(_EmptyPList & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

pylox (https://github.com/sco1/pylox)
- pylox/interpreter.py:127:20: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:127:20: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:142:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:142:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:152:25: error[invalid-argument-type] Argument is incorrect: Expected `Environment`, found `Unknown | Environment | None`
+ pylox/interpreter.py:152:25: error[invalid-argument-type] Argument is incorrect: Expected `Environment`, found `Environment | None | Unknown`
- pylox/interpreter.py:160:33: warning[possibly-missing-attribute] Attribute `enclosing` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:160:33: warning[possibly-missing-attribute] Attribute `enclosing` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:162:9: warning[possibly-missing-attribute] Attribute `assign` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:162:9: warning[possibly-missing-attribute] Attribute `assign` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:168:38: error[invalid-argument-type] Argument is incorrect: Expected `Environment`, found `Unknown | Environment | None`
+ pylox/interpreter.py:168:38: error[invalid-argument-type] Argument is incorrect: Expected `Environment`, found `Environment | None | Unknown`
- pylox/interpreter.py:169:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:169:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:184:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:184:9: warning[possibly-missing-attribute] Attribute `define` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:252:13: warning[possibly-missing-attribute] Attribute `assign_at` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:252:13: warning[possibly-missing-attribute] Attribute `assign_at` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:359:32: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:359:32: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Environment | None | Unknown`
- pylox/interpreter.py:363:32: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Unknown | Environment | None`
+ pylox/interpreter.py:363:32: warning[possibly-missing-attribute] Attribute `get_at` may be missing on object of type `Environment | None | Unknown`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/pkgconfig.py:745:67: error[invalid-argument-type] Argument to bound method `_generate_pkgconfig_file` is incorrect: Expected `str`, found `str | None | Unknown`
+ mesonbuild/modules/pkgconfig.py:745:67: error[invalid-argument-type] Argument to bound method `_generate_pkgconfig_file` is incorrect: Expected `str`, found `None | str | Unknown`
- mesonbuild/modules/pkgconfig.py:754:67: error[invalid-argument-type] Argument to bound method `_generate_pkgconfig_file` is incorrect: Expected `str`, found `str | None | Unknown`
+ mesonbuild/modules/pkgconfig.py:754:67: error[invalid-argument-type] Argument to bound method `_generate_pkgconfig_file` is incorrect: Expected `str`, found `None | str | Unknown`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/command/build_ext.py:282:30: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `(Unknown & ~None) | list[Unknown]`
+ setuptools/_distutils/command/build_ext.py:282:30: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `list[Unknown] | (Unknown & ~None)`
- setuptools/_distutils/command/build_ext.py:622:17: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["-c++"]` and `Unknown | None | list[Unknown]`
+ setuptools/_distutils/command/build_ext.py:622:17: error[unsupported-operator] Operator `in` is not supported between objects of type `Literal["-c++"]` and `None | list[Unknown] | Unknown`
- setuptools/_distutils/command/build_ext.py:643:25: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Unknown | str]`, found `Unknown | None | list[Unknown]`
+ setuptools/_distutils/command/build_ext.py:643:25: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Unknown | str]`, found `None | list[Unknown] | Unknown`

archinstall (https://github.com/archlinux/archinstall)
- archinstall/lib/profile/profiles_handler.py:144:10: error[invalid-return-type] Return type does not match returned value: expected `list[Profile]`, found `Unknown | Divergent | list[Profile] | None`
+ archinstall/lib/profile/profiles_handler.py:144:10: error[invalid-return-type] Return type does not match returned value: expected `list[Profile]`, found `None | list[Profile] | Unknown | Divergent`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/reshape/merge.py:2435:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Divergent] | list[Unknown | None | list[Divergent]]`
+ pandas/core/reshape/merge.py:2435:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `None | list[Divergent] | Unknown | list[Unknown | None | list[Divergent]]`
- pandas/core/reshape/merge.py:2439:24: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None | list[Divergent] | list[Unknown | None | list[Divergent]]` and `list[Unknown]`
+ pandas/core/reshape/merge.py:2439:24: error[unsupported-operator] Operator `+` is not supported between objects of type `None | list[Divergent] | Unknown | list[Unknown | None | list[Divergent]]` and `list[Unknown]`

xarray (https://github.com/pydata/xarray)
- xarray/backends/zarr.py:1242:21: error[invalid-argument-type] Argument to function `grid_rechunk` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | str | slice[Any, Any, Any], ...]`
+ xarray/backends/zarr.py:1242:21: error[invalid-argument-type] Argument to function `grid_rechunk` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[str | slice[Any, Any, Any] | Unknown, ...]`
- xarray/backends/zarr.py:1259:21: error[invalid-argument-type] Argument to function `validate_grid_chunks_alignment` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | str | slice[Any, Any, Any], ...]`
+ xarray/backends/zarr.py:1259:21: error[invalid-argument-type] Argument to function `validate_grid_chunks_alignment` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[str | slice[Any, Any, Any] | Unknown, ...]`
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8875:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/scintilla/formatter.py:48:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None`
+ Pythonwin/pywin/scintilla/formatter.py:48:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `None | Unknown`
- Pythonwin/pywin/scintilla/formatter.py:67:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None`
+ Pythonwin/pywin/scintilla/formatter.py:67:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `None | Unknown`
- win32/Lib/win32rcparser.py:223:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:223:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:229:16: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `Unknown | str | None`
+ win32/Lib/win32rcparser.py:229:16: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `None | str | Unknown`
- win32/Lib/win32rcparser.py:389:19: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `Unknown | str | None`
+ win32/Lib/win32rcparser.py:389:19: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `None | str | Unknown`
- win32/Lib/win32rcparser.py:400:19: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `Unknown | str | None`
+ win32/Lib/win32rcparser.py:400:19: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `None | str | Unknown`
- win32/Lib/win32rcparser.py:404:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:404:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:407:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:407:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:410:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:410:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:413:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:413:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:485:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | str | None`
+ win32/Lib/win32rcparser.py:485:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | str | Unknown`
- win32/Lib/win32rcparser.py:512:43: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `(Unknown & ~Literal["END"] & ~Literal["-"]) | (str & ~Literal["END"] & ~Literal["-"]) | None`
+ win32/Lib/win32rcparser.py:512:43: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `None | (str & ~Literal["END"] & ~Literal["-"]) | (Unknown & ~Literal["END"] & ~Literal["-"])`
- win32/Lib/win32rcparser.py:520:37: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `(Unknown & ~Literal["END"] & ~Literal["-"]) | (str & ~Literal["END"] & ~Literal["-"]) | None`
+ win32/Lib/win32rcparser.py:520:37: warning[possibly-missing-attribute] Attribute `isdigit` may be missing on object of type `None | (str & ~Literal["END"] & ~Literal["-"]) | (Unknown & ~Literal["END"] & ~Literal["-"])`
- win32/Lib/win32rcparser.py:553:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `(Unknown & ~Literal["END"]) | (str & ~Literal["END"]) | None`
+ win32/Lib/win32rcparser.py:553:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `None | (str & ~Literal["END"]) | (Unknown & ~Literal["END"])`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:861:33: warning[possibly-missing-attribute] Attribute `tolist` may be missing on object of type `Unknown | list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:861:33: warning[possibly-missing-attribute] Attribute `tolist` may be missing on object of type `list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:862:38: warning[possibly-missing-attribute] Attribute `tolist` may be missing on object of type `Unknown | list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ sklearn/ensemble/_hist_gradient_boosting/gradient_boosting.py:862:38: warning[possibly-missing-attribute] Attribute `tolist` may be missing on object of type `list[Unknown] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:640:28: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/_stochastic_gradient.py:643:32: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/_stochastic_gradient.py:640:28: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/_stochastic_gradient.py:643:32: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/_stochastic_gradient.py:777:30: warning[possibly-missing-attribute] Attribute `reshape` may be missing on object of type `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/_stochastic_gradient.py:777:30: warning[possibly-missing-attribute] Attribute `reshape` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/_stochastic_gradient.py:1686:37: warning[possibly-missing-attribute] Attribute `T` may be missing on object of type `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:1686:37: warning[possibly-missing-attribute] Attribute `T` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:1739:58: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | None | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:1739:58: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:2358:58: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | None | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:2358:58: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:2447:28: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:2447:28: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:2450:32: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:2450:32: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/_stochastic_gradient.py:2632:40: warning[possibly-missing-attribute] Attribute `T` may be missing on object of type `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/_stochastic_gradient.py:2632:40: warning[possibly-missing-attribute] Attribute `T` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_huber.py:171:44: error[invalid-argument-type] Argument to function `assert_array_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/tests/test_huber.py:171:44: error[invalid-argument-type] Argument to function `assert_array_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_passive_aggressive.py:154:29: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_passive_aggressive.py:154:29: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/tests/test_passive_aggressive.py:215:25: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_passive_aggressive.py:215:25: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/tests/test_passive_aggressive.py:215:36: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_passive_aggressive.py:215:36: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/tests/test_passive_aggressive.py:216:25: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_passive_aggressive.py:216:25: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/tests/test_passive_aggressive.py:216:36: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_passive_aggressive.py:216:36: error[invalid-argument-type] Argument to function `assert_almost_equal` is incorrect: Expected `_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]] | _NestedSequence[_SupportsArray[dtype[numpy.bool[builtins.bool] | number[Any, int | float | complex]]]] | int | ... omitted 5 union elements`, found `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/linear_model/tests/test_passive_aggressive.py:291:29: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `Unknown | None | ndarray[tuple[Any, ...], dtype[Unknown]] | ndarray[tuple[int], dtype[Unknown]] | csr_matrix[Any]`
+ sklearn/linear_model/tests/test_passive_aggressive.py:291:29: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ndarray[tuple[Any, ...], dtype[Unknown]] | Unknown`
- sklearn/linear_model/tests/test_perceptron.py:60:39: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `Unknown | None | ndarray[tuple[int, int], dtype[Unknown]] | ... omitted 3 union elements`
+ sklearn/linear_model/tests/test_perceptron.py:60:39: warning[possibly-missing-attribute] Attribute `ravel` may be missing on object of type `None | csr_matrix[Any] | ndarray[tuple[int], dtype[Unknown]] | ... omitted 3 union elements`
- sklearn/model_selection/_search_successive_halving.py:292:17: error[unsupported-operator] Operator `//` is not supported between objects of type `Unknown | Literal["auto"]` and `Unknown | Literal["exhaust", 1]`
+ sklearn/model_selection/_search_successive_halving.py:292:17: error[unsupported-operator] Operator `//` is not supported between objects of type `Unknown | Literal["auto"]` and `Literal[1, "exhaust"] | Unknown`
- sklearn/model_selection/_search_successive_halving.py:1094:39: error[unsupported-operator] Operator `//` is not supported between objects of type `Unknown | Literal["auto"]` and `Unknown | Literal["exhaust", 1]`
+ sklearn/model_selection/_search_successive_halving.py:1094:39: error[unsupported-operator] Operator `//` is not supported between objects of type `Unknown | Literal["auto"]` and `Literal[1, "exhaust"] | Unknown`
- sklearn/tree/_classes.py:540:24: warning[possibly-missing-attribute]

... (truncated 166 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-01 11:13_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 11:17_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Closed by @mtshiba on 2025-12-01 11:19_

---

_Reopened by @mtshiba on 2025-12-01 11:19_

---

_Comment by @mtshiba on 2025-12-01 11:37_

Hmm, at least, this approach alone doesn't seem to eliminate the instability of the diagnostic itself, rather than the instability of the output type.

1st: https://github.com/astral-sh/ruff/actions/runs/19820273428/job/56780817916?pr=21722

```sh
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics
```

2nd: https://github.com/astral-sh/ruff/actions/runs/19820836584/job/56782606719?pr=21722

```sh
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics
```

---

_Comment by @MichaReiser on 2025-12-01 11:39_

> Hmm, at least, this approach alone doesn't seem to eliminate the instability of the diagnostic itself, rather than the instability of the output type.

I think you need to create a PR targeting this PR to verify your change because mypy primer compares main vs this branch. What you want is to compare the branch to itself to see if there are any remaining instabilities

---

_Comment by @mtshiba on 2025-12-01 11:46_

> I think you need to create a PR targeting this PR to verify your change because mypy primer compares main vs this branch. What you want is to compare the branch to itself to see if there are any remaining instabilities

Ah, I see, main is unstable, so it can't be used for comparison.

---

_Comment by @codspeed-hq[bot] on 2025-12-06 06:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Asort-recursive-union?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21722 will **not alter performance**

<sub>Comparing <code>mtshiba:sort-recursive-union</code> (f46b29d) with <code>main</code> (0bd7a94)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @MichaReiser on 2025-12-10 16:34_

The pydantic speedup is interesting

I added a CI job in https://github.com/astral-sh/ruff/pull/21864 that should make it easier to verify a fix.

---

_Comment by @mtshiba on 2025-12-12 06:07_

> I added a CI job in #21864 that should make it easier to verify a fix.

Thanks!

> The pydantic speedup is interesting

There seems to be a trade-off between the cost of sorting unions and the cost of recalculation in the fixed point iterations, and in many cases the sorting cost is higher, but in the case of pydantic, the improvement in the convergence speed of the fixed point iterations seems to outweigh it.
We may be able to improve overall performance by adding a handling that sorts all unions if the fixed point iterations don't converge within a few cycles.


---

_Comment by @mtshiba on 2025-12-12 08:45_

Sorting only the union types that appear in cycle heads does not seem to eliminate the non-determinism, likely because the non-determinism in query execution order also makes it non-deterministic which queries become cycle heads.

---
