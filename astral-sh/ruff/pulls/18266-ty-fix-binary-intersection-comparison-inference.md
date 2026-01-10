```yaml
number: 18266
title: "[ty] Fix binary intersection comparison inference logic"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-binary-intersection-operation
created_at: 2025-05-23T01:53:32Z
updated_at: 2025-05-23T13:26:34Z
url: https://github.com/astral-sh/ruff/pull/18266
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Fix binary intersection comparison inference logic

---

_Pull request opened by @InSyncWithFoo on 2025-05-23 01:53_

## Summary

Resolves https://github.com/astral-sh/ty/issues/485.

`infer_binary_intersection_type_comparison()` now checks for all positive members before concluding that an operation is unsupported for a given intersection type.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-23 01:53_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-23 01:53_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-23 01:53_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-23 01:53_

---

_Label `ty` added by @AlexWaygood on 2025-05-23 01:54_

---

_Comment by @github-actions[bot] on 2025-05-23 01:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- error[unsupported-operator] beartype/_cave/_cavemap.py:144:18: Operator `in` is not supported for types `type` and `str`, in comparing `type` with `(str & tuple[Unknown, ...] & ~type & ~AlwaysFalsy) | (@Todo(Inference of subscript on special form) & tuple[Unknown, ...] & ~type & ~AlwaysFalsy)`
- Found 551 diagnostics
+ Found 550 diagnostics

stone (https://github.com/dropbox/stone)
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:305:20: Operator `>=` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:309:20: Operator `>` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:367:20: Operator `>=` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:371:20: Operator `>` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:430:20: Operator `>=` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] stone/backends/python_rsrc/stone_validators.py:434:20: Operator `>` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- Found 179 diagnostics
+ Found 173 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[unsupported-operator] tests/language/test_visitor.py:44:16: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `(Unknown & int) | (Unknown & str & int)`
- error[unsupported-operator] tests/language/test_visitor.py:44:21: Operator `<=` is not supported for types `str` and `int`, in comparing `(Unknown & int) | (Unknown & str & int)` with `int`
- error[unsupported-operator] tests/language/test_visitor.py:64:24: Operator `<=` is not supported for types `int` and `str`, in comparing `Literal[0]` with `(@Todo(Type::Intersection.call()) & int) | (@Todo(Type::Intersection.call()) & str & int)`
- error[unsupported-operator] tests/language/test_visitor.py:64:29: Operator `<=` is not supported for types `str` and `int`, in comparing `(@Todo(Type::Intersection.call()) & int) | (@Todo(Type::Intersection.call()) & str & int)` with `int`
- Found 411 diagnostics
+ Found 407 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[unsupported-operator] tests/test_multiple_interpreters.py:67:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["does not support loading in subinterpreters"]` with `(Unknown & None) | @Todo(map_with_boundness: intersections with negative contributions) | str`
- Found 264 diagnostics
+ Found 263 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[unsupported-operator] ignite/engine/events.py:90:81: Operator `>` is not supported for types `Integral` and `int`, in comparing `int & Integral` with `Literal[0]`
+ error[unsupported-operator] ignite/engine/events.py:94:57: Operator `>` is not supported for types `list[Unknown]` and `int`, in comparing `(int & Integral) | (list[Unknown] & Integral)` with `Literal[0]`
- error[unsupported-operator] ignite/engine/events.py:94:57: Operator `>` is not supported for types `Integral` and `int`, in comparing `(int & Integral) | (list[Unknown] & Integral)` with `Literal[0]`
- error[unsupported-operator] ignite/engine/events.py:101:83: Operator `>=` is not supported for types `Integral` and `int`, in comparing `int & Integral` with `Literal[0]`
- error[unsupported-operator] ignite/engine/events.py:104:81: Operator `>=` is not supported for types `Integral` and `int`, in comparing `int & Integral` with `Literal[0]`
- error[unsupported-operator] ignite/handlers/param_scheduler.py:1128:13: Operator `>` is not supported for types `Integral` and `int`, in comparing `int & Integral` with `Literal[1]`
- error[unsupported-operator] ignite/handlers/param_scheduler.py:1156:20: Operator `>` is not supported for types `Integral` and `int`, in comparing `int & Integral` with `Literal[2]`
- error[unsupported-operator] ignite/metrics/confusion_matrix.py:253:64: Operator `<=` is not supported for types `int` and `Integral`, in comparing `Literal[0]` with `int & Integral`
- error[unsupported-operator] ignite/metrics/confusion_matrix.py:409:64: Operator `<=` is not supported for types `int` and `Integral`, in comparing `Literal[0]` with `int & Integral`
- Found 2229 diagnostics
+ Found 2222 diagnostics

vision (https://github.com/pytorch/vision)
- error[unsupported-operator] torchvision/transforms/functional.py:373:16: Operator `<=` is not supported for types `int` and `list[int]`, in comparing `int` with `(list[int] & int) | @Todo(map_with_boundness: intersections with negative contributions)`
- error[unsupported-operator] torchvision/transforms/transforms.py:1208:16: Operator `<` is not supported for types `Number` and `int`, in comparing `Unknown & Number` with `Literal[0]`
- error[unsupported-operator] torchvision/transforms/transforms.py:1779:16: Operator `<=` is not supported for types `Number` and `int`, in comparing `(Unknown & Number) | (tuple[float, float] & Number)` with `Literal[0]`
+ error[unsupported-operator] torchvision/transforms/transforms.py:1779:16: Operator `<=` is not supported for types `tuple[float, float]` and `Literal[0]`, in comparing `(Unknown & Number) | (tuple[float, float] & Number)` with `Literal[0]`
- error[unsupported-operator] torchvision/transforms/transforms.py:1842:12: Operator `<` is not supported for types `Number` and `int`, in comparing `Unknown & Number` with `Literal[0]`
- Found 1843 diagnostics
+ Found 1840 diagnostics

paasta (https://github.com/yelp/paasta)
- error[unsupported-operator] paasta_tools/api/views/autoscaler.py:84:10: Operator `<` is not supported for types `int` and `None`, in comparing `Unknown & int` with `int | None`
- Found 935 diagnostics
+ Found 934 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[unsupported-operator] schema_salad/jsonld_context.py:210:16: Operator `in` is not supported for types `str` and `int`, in comparing `str` with `(CommentedMap & MutableMapping[Unknown, Unknown]) | (int & MutableMapping[Unknown, Unknown]) | (float & MutableMapping[Unknown, Unknown]) | (str & MutableMapping[Unknown, Unknown]) | (CommentedSeq & MutableMapping[Unknown, Unknown])`
- Found 251 diagnostics
+ Found 250 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[unsupported-operator] mitmproxy/io/compat.py:519:21: Operator `>` is not supported for types `tuple[Unknown, ...]` and `int`, in comparing `(Any & int) | (tuple[Unknown, ...] & int)` with `Unknown | Literal[21]`
- Found 2112 diagnostics
+ Found 2111 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- error[unsupported-operator] github/AdvisoryCredit.py:94:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `(Unknown & dict[Unknown, Unknown]) | (Unknown & AdvisoryCredit & dict[Unknown, Unknown])`
- error[unsupported-operator] github/AdvisoryCredit.py:95:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `(Unknown & dict[Unknown, Unknown]) | (Unknown & AdvisoryCredit & dict[Unknown, Unknown])`
- error[unsupported-operator] github/AdvisoryCredit.py:106:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["login"]` with `(Unknown & dict[Unknown, Unknown]) | (Unknown & AdvisoryCredit & dict[Unknown, Unknown])`
- error[unsupported-operator] github/AdvisoryCredit.py:107:20: Operator `in` is not supported for types `str` and `AdvisoryCredit`, in comparing `Literal["type"]` with `(Unknown & dict[Unknown, Unknown]) | (Unknown & AdvisoryCredit & dict[Unknown, Unknown])`
- Found 316 diagnostics
+ Found 312 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[unsupported-operator] cloudinit/sources/DataSourceAliYun.py:366:35: Operator `>=` is not supported for types `None` and `int`, in comparing `Unknown | (Unknown & int) | (Unknown & None)` with `Literal[400]`
- error[unsupported-operator] tests/unittests/sources/helpers/test_ec2.py:279:38: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["meta-data/tags/"]` with `Unknown | (Unknown & str) | (Unknown & None)`
- Found 717 diagnostics
+ Found 715 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- error[unsupported-operator] mypy_django_plugin/django/context.py:383:18: Operator `not in` is not supported for types `str` and `type[Model]`, in comparing `Literal["."]` with `(type[Model] & str) | (@Todo(map_with_boundness: intersections with negative contributions) & str & ~Literal["self"])`
- Found 473 diagnostics
+ Found 472 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[unsupported-operator] sphinx/builders/html/__init__.py:471:40: Operator `not in` is not supported for types `str` and `Literal[True]`, in comparing `str` with `(Unknown & Literal[True]) | frozenset[Unknown]`
- error[unsupported-operator] sphinx/writers/latex.py:567:40: Operator `not in` is not supported for types `str` and `Literal[True]`, in comparing `str` with `(Unknown & Literal[True]) | frozenset[Unknown]`
- error[unsupported-operator] sphinx/writers/texinfo.py:500:40: Operator `not in` is not supported for types `str` and `Literal[True]`, in comparing `str` with `(Unknown & Literal[True]) | frozenset[Unknown]`
- Found 660 diagnostics
+ Found 657 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[unsupported-operator] static_frame/profile/__main__.py:2502:42: Operator `in` is not supported for types `@Todo(Type::Intersection.call())` and `None`, in comparing `@Todo(Type::Intersection.call())` with `(Any & dict[Unknown, Unknown]) | (Any & None)`
- Found 2063 diagnostics
+ Found 2062 diagnostics

apprise (https://github.com/caronc/apprise)
- error[unsupported-operator] test/test_api.py:452:12: Operator `in` is not supported for types `str` and `NotifyBase`, in comparing `Literal["good"]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:453:12: Operator `not in` is not supported for types `str` and `NotifyBase`, in comparing `Literal["bad"]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:458:12: Operator `in` is not supported for types `list[Unknown]` and `NotifyBase`, in comparing `list[Unknown]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:459:12: Operator `in` is not supported for types `set[Unknown]` and `NotifyBase`, in comparing `set[Unknown]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:460:12: Operator `in` is not supported for types `tuple[Literal["bad"], Literal["good"]]` and `NotifyBase`, in comparing `tuple[Literal["bad"], Literal["good"]]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:497:12: Operator `in` is not supported for types `str` and `NotifyBase`, in comparing `Literal["good"]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:498:12: Operator `not in` is not supported for types `str` and `NotifyBase`, in comparing `Literal["bad"]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:503:12: Operator `in` is not supported for types `list[Unknown]` and `NotifyBase`, in comparing `list[Unknown]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:504:12: Operator `in` is not supported for types `set[Unknown]` and `NotifyBase`, in comparing `set[Unknown]` with `Unknown & NotifyBase`
- error[unsupported-operator] test/test_api.py:505:12: Operator `in` is not supported for types `tuple[Literal["bad"], Literal["good"]]` and `NotifyBase`, in comparing `tuple[Literal["bad"], Literal["good"]]` with `Unknown & NotifyBase`
- Found 3704 diagnostics
+ Found 3694 diagnostics

sympy (https://github.com/sympy/sympy)
- error[unsupported-operator] sympy/core/relational.py:354:56: Operator `>` is not supported for types `Basic` and `Basic`, in comparing `(Unknown & Basic) | Unknown` with `(Unknown & Basic) | Unknown`
- error[unsupported-operator] sympy/holonomic/holonomic.py:1760:32: Operator `in` is not supported for types `Unknown` and `<Protocol with members '__iter__'>`, in comparing `Unknown` with `(Unknown & <Protocol with members '__iter__'>) | list[Unknown]`
- Found 18754 diagnostics
+ Found 18752 diagnostics

scipy (https://github.com/scipy/scipy)
- error[unsupported-operator] scipy/special/_multiufuncs.py:327:17: Operator `>=` is not supported for types `Integral` and `int`, in comparing `Unknown & Integral` with `Literal[0]`
- error[unsupported-operator] scipy/special/_multiufuncs.py:331:12: Operator `<=` is not supported for types `int` and `Integral`, in comparing `Literal[0]` with `Unknown & Integral`
- error[unsupported-operator] scipy/special/_multiufuncs.py:407:12: Operator `<=` is not supported for types `int` and `Integral`, in comparing `Literal[0]` with `Unknown & Integral`
- error[unsupported-operator] scipy/stats/_mstats_basic.py:2255:12: Operator `>` is not supported for types `tuple[float, float]` and `float`, in comparing `(@Todo(Type::Intersection.call()) & ~None) | (Unknown & float & ~tuple[Unknown, ...]) | (tuple[float, float] & float & ~tuple[Unknown, ...])` with `float`
- error[unsupported-operator] scipy/stats/_mstats_basic.py:2255:26: Operator `<` is not supported for types `tuple[float, float]` and `Literal[0]`, in comparing `(@Todo(Type::Intersection.call()) & ~None) | (Unknown & float & ~tuple[Unknown, ...]) | (tuple[float, float] & float & ~tuple[Unknown, ...])` with `Literal[0]`
- error[unsupported-operator] scipy/stats/_mstats_basic.py:2258:12: Operator `>` is not supported for types `tuple[float, float]` and `float`, in comparing `(@Todo(Type::Intersection.call()) & ~None) | (Unknown & float & ~tuple[Unknown, ...]) | (tuple[float, float] & float & ~tuple[Unknown, ...])` with `float`
- error[unsupported-operator] scipy/stats/_mstats_basic.py:2258:26: Operator `<` is not supported for types `tuple[float, float]` and `Literal[0]`, in comparing `(@Todo(Type::Intersection.call()) & ~None) | (Unknown & float & ~tuple[Unknown, ...]) | (tuple[float, float] & float & ~tuple[Unknown, ...])` with `Literal[0]`
- Found 7819 diagnostics
+ Found 7812 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[unsupported-operator] manticore/wasm/structure.py:1580:32: Operator `>=` is not supported for types `Expression` and `int`, in comparing `@Todo(Inference of subscript on special form) & Expression` with `Literal[0]`
- error[unsupported-operator] manticore/wasm/structure.py:1580:42: Operator `<` is not supported for types `Expression` and `U32`, in comparing `@Todo(Inference of subscript on special form) & Expression` with `U32`
- error[unsupported-operator] manticore/wasm/structure.py:1661:18: Operator `>=` is not supported for types `Expression` and `int`, in comparing `@Todo(Inference of subscript on special form) & Expression` with `Literal[0]`
- error[unsupported-operator] manticore/wasm/structure.py:1661:32: Operator `<` is not supported for types `Expression` and `int`, in comparing `@Todo(Inference of subscript on special form) & Expression` with `int`
- Found 1236 diagnostics
+ Found 1232 diagnostics

```
</details>


---

_Comment by @InSyncWithFoo on 2025-05-23 03:36_

The ecosystem changes are as expected: most changes are removals of false positives.

These are the two "added" errors:

```diff
ignite (https://github.com/pytorch/ignite)
+ error[unsupported-operator] ignite/engine/events.py:94:57: Operator `>` is not supported for types `list[Unknown]` and `int`, in comparing `(int & Integral) | (list[Unknown] & Integral)` with `Literal[0]`

vision (https://github.com/pytorch/vision)
+ error[unsupported-operator] torchvision/transforms/transforms.py:1779:16: Operator `<=` is not supported for types `tuple[float, float]` and `Literal[0]`, in comparing `(Unknown & Number) | (tuple[float, float] & Number)` with `Literal[0]`
```

Due to the new logic, these errors have slightly confusing messages, but the underlying problem is that [`Integral > int` and `Number <= int` are considered unsupported](https://play.ty.dev/44bed38d-afce-4ab8-826b-ac8438499b5c).



---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6641 on 2025-05-23 10:28_

I think we should actually show an error if we saw no element that supports the operation (unless the operator is supported on `object`).

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6657 on 2025-05-23 10:28_

It would be great if we could get rid of this by employing a "make illegal states unrepresentable" strategy.

---

_@sharkdp requested changes on 2025-05-23 10:30_

Thank you!

I will push a small update with a refactoring (using a bespoke `State` enum to get rid of the unreachable branch), and a functional change where we should error if there is no positive element in the intersection.

---

_@sharkdp approved on 2025-05-23 10:55_

---

_Merged by @sharkdp on 2025-05-23 10:55_

---

_Closed by @sharkdp on 2025-05-23 10:55_

---

_Branch deleted on 2025-05-23 13:26_

---
