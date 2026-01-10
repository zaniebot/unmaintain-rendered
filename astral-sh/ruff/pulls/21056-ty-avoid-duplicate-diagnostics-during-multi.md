```yaml
number: 21056
title: "[ty] Avoid duplicate diagnostics during multi-inference of standalone expressions"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/duplicate-diagnostics
created_at: 2025-10-24T05:37:09Z
updated_at: 2025-10-24T17:21:41Z
url: https://github.com/astral-sh/ruff/pull/21056
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Avoid duplicate diagnostics during multi-inference of standalone expressions

---

_Pull request opened by @ibraheemdev on 2025-10-24 05:37_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1428.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-24 05:37_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-24 05:37_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-24 05:37_

---

_Label `ty` added by @ibraheemdev on 2025-10-24 05:37_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-24 05:37_

---

_Comment by @github-actions[bot] on 2025-10-24 05:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 05:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:79: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
- lib/spack/spack/vendor/pyrsistent/_pdeque.py:95:79: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `_maxlen`
- Found 7771 diagnostics
+ Found 7769 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/rtcpeerconnection.py:985:42: warning[possibly-missing-attribute] Attribute `role` may be missing on object of type `RTCDtlsParameters | None`
- src/aiortc/rtcpeerconnection.py:985:42: warning[possibly-missing-attribute] Attribute `role` may be missing on object of type `RTCDtlsParameters | None`
- Found 189 diagnostics
+ Found 187 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[0]`
- tests/GithubRetry.py:102:53: error[unsupported-operator] Operator `>` is not supported for types `None` and `int`, in comparing `Unknown | None` with `Literal[0]`
- Found 352 diagnostics
+ Found 350 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
- sockeye/output_handler.py:254:80: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[Any] | None`
- Found 407 diagnostics
+ Found 405 diagnostics

flake8 (https://github.com/pycqa/flake8)
- tests/unit/plugins/pycodestyle_test.py:30:48: error[unresolved-attribute] Object of type `ModuleType` has no attribute `lines`
- tests/unit/plugins/pycodestyle_test.py:30:48: error[unresolved-attribute] Object of type `ModuleType` has no attribute `lines`
- Found 40 diagnostics
+ Found 38 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pyspark/container.py:237:34: error[unresolved-attribute] Object of type `BaseSchemaBackend` has no attribute `get_regex_columns`
- pandera/api/pyspark/container.py:237:34: error[unresolved-attribute] Object of type `BaseSchemaBackend` has no attribute `get_regex_columns`
- pandera/api/pyspark/container.py:237:34: error[unresolved-attribute] Object of type `BaseSchemaBackend` has no attribute `get_regex_columns`
- pandera/api/pyspark/container.py:237:34: error[unresolved-attribute] Object of type `BaseSchemaBackend` has no attribute `get_regex_columns`
- Found 1635 diagnostics
+ Found 1631 diagnostics

vision (https://github.com/pytorch/vision)
- references/detection/coco_utils.py:165:60: warning[possibly-unresolved-reference] Name `keypoints` used when possibly not defined
- Found 1474 diagnostics
+ Found 1473 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/mixture/_base.py:470:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:470:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:471:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `covariances_`
- sklearn/mixture/_base.py:471:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `covariances_`
- sklearn/mixture/_base.py:483:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:483:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:494:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:494:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `means_`
- sklearn/mixture/_base.py:495:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `covariances_`
- sklearn/mixture/_base.py:495:43: error[unresolved-attribute] Object of type `Self@sample` has no attribute `covariances_`
- Found 2477 diagnostics
+ Found 2467 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/cpu/abstractcpu.py:1060:63: error[unresolved-attribute] Object of type `Instruction` has no attribute `bytes`
- manticore/native/cpu/abstractcpu.py:1060:63: error[unresolved-attribute] Object of type `Instruction` has no attribute `bytes`
- Found 1479 diagnostics
+ Found 1477 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/backend/backends.py:1585:49: warning[possibly-missing-attribute] Attribute `get_build_command` may be missing on object of type `Environment | None`
- mesonbuild/backend/backends.py:1585:49: warning[possibly-missing-attribute] Attribute `get_build_command` may be missing on object of type `Environment | None`
- mesonbuild/rewriter.py:940:130: warning[possibly-missing-attribute] Attribute `extra_files` may be missing on object of type `IntrospectionBuildTarget | None`
- mesonbuild/rewriter.py:940:130: warning[possibly-missing-attribute] Attribute `extra_files` may be missing on object of type `IntrospectionBuildTarget | None`
- Found 1644 diagnostics
+ Found 1640 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/server/schemas/schedules.py:545:62: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_rrule`
- src/prefect/server/schemas/schedules.py:545:62: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_rrule`
- src/prefect/server/schemas/schedules.py:548:62: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_exrule`
- src/prefect/server/schemas/schedules.py:548:62: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_exrule`
- src/prefect/server/schemas/schedules.py:554:61: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_rdate`
- src/prefect/server/schemas/schedules.py:554:61: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_rdate`
- src/prefect/server/schemas/schedules.py:559:63: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_exdate`
- src/prefect/server/schemas/schedules.py:559:63: error[unresolved-attribute] Object of type `rruleset & ~rrule` has no attribute `_exdate`
- Found 3264 diagnostics
+ Found 3256 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/command/editable_wheel.py:261:32: error[call-non-callable] Object of type `object` is not callable
- setuptools/command/editable_wheel.py:261:32: error[call-non-callable] Object of type `object` is not callable
- Found 1165 diagnostics
+ Found 1163 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/parametertools.py:3422:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `int | float`
- hydpy/core/parametertools.py:3422:56: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `int | float`
- Found 663 diagnostics
+ Found 661 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/pymongo/client.py:340:68: warning[possibly-unresolved-reference] Name `cursor` used when possibly not defined
- ddtrace/contrib/internal/unittest/patch.py:671:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` may be missing on object of type `CIVisibility | None`
- ddtrace/contrib/internal/unittest/patch.py:671:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` may be missing on object of type `CIVisibility | None`
- ddtrace/contrib/internal/unittest/patch.py:718:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` may be missing on object of type `CIVisibility | None`
- ddtrace/contrib/internal/unittest/patch.py:718:19: warning[possibly-missing-attribute] Attribute `_collect_coverage_enabled` may be missing on object of type `CIVisibility | None`
- ddtrace/vendor/psutil/_compat.py:151:50: warning[possibly-unresolved-reference] Name `sorted_items` used when possibly not defined
- ddtrace/vendor/psutil/_compat.py:151:50: warning[possibly-unresolved-reference] Name `sorted_items` used when possibly not defined
- tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:36:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
- tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:36:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
- tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:71:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
- tests/appsec/iast_packages/packages/pkg_exceptiongroup.py:71:83: warning[possibly-unresolved-reference] Name `caught_exceptions` used when possibly not defined
- Found 7998 diagnostics
+ Found 7987 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/common/deferred.py:501:55: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `values`
- ibis/common/deferred.py:501:55: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `values`
- Found 4193 diagnostics
+ Found 4191 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:1517:39: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2476 diagnostics
+ Found 2475 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/codegen/ast.py:1403:50: error[unresolved-attribute] Object of type `Self@_sympystr` has no attribute `parameters`
- sympy/codegen/ast.py:1403:50: error[unresolved-attribute] Object of type `Self@_sympystr` has no attribute `parameters`
- sympy/combinatorics/partitions.py:574:43: error[unresolved-attribute] Object of type `Self@as_ferrers` has no attribute `partition`
- sympy/combinatorics/partitions.py:574:43: error[unresolved-attribute] Object of type `Self@as_ferrers` has no attribute `partition`
- sympy/core/logic.py:265:54: error[unresolved-attribute] Object of type `Self@__str__` has no attribute `args`
- sympy/core/logic.py:265:54: error[unresolved-attribute] Object of type `Self@__str__` has no attribute `args`
- sympy/functions/combinatorial/numbers.py:3193:61: warning[possibly-unresolved-reference] Name `s` used when possibly not defined
- sympy/matrices/expressions/matexpr.py:840:41: error[unresolved-attribute] Object of type `Self@rank` has no attribute `first`
- sympy/matrices/expressions/matexpr.py:842:41: error[unresolved-attribute] Object of type `Self@rank` has no attribute `second`
- Found 13704 diagnostics
+ Found 13695 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
- homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
- homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
- homeassistant/components/bmw_connected_drive/notify.py:59:44: warning[possibly-unresolved-reference] Name `coordinator` used when possibly not defined
- homeassistant/components/caldav/todo.py:84:28: warning[possibly-unresolved-reference] Name `todo` used when possibly not defined
- homeassistant/components/konnected/__init__.py:355:28: warning[possibly-unresolved-reference] Name `payload` used when possibly not defined
- homeassistant/components/konnected/__init__.py:355:28: warning[possibly-unresolved-reference] Name `payload` used when possibly not defined
- homeassistant/components/sensor/__init__.py:607:59: warning[possibly-unresolved-reference] Name `classes` used when possibly not defined
- homeassistant/components/sensor/__init__.py:607:59: warning[possibly-unresolved-reference] Name `classes` used when possibly not defined
- homeassistant/helpers/entity_platform.py:974:17: warning[possibly-unresolved-reference] Name `entry` used when possibly not defined
- homeassistant/helpers/entity_platform.py:974:17: warning[possibly-unresolved-reference] Name `entry` used when possibly not defined
- Found 14116 diagnostics
+ Found 14105 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/_util.py:1074:36: error[call-non-callable] Object of type `None` is not callable
- scipy/_lib/_util.py:1074:36: error[call-non-callable] Object of type `None` is not callable
- scipy/spatial/tests/test_kdtree.py:591:41: error[unresolved-attribute] Object of type `Self@test_one_radius` has no attribute `T1`
- scipy/spatial/tests/test_kdtree.py:591:41: error[unresolved-attribute] Object of type `Self@test_one_radius` has no attribute `T1`
- scipy/spatial/tests/test_kdtree.py:591:65: error[unresolved-attribute] Object of type `Self@test_one_radius` has no attribute `T2`
- scipy/spatial/tests/test_kdtree.py:591:65: error[unresolved-attribute] Object of type `Self@test_one_radius` has no attribute `T2`
- scipy/spatial/tests/test_kdtree.py:596:41: error[unresolved-attribute] Object of type `Self@test_large_radius` has no attribute `T1`
- scipy/spatial/tests/test_kdtree.py:596:41: error[unresolved-attribute] Object of type `Self@test_large_radius` has no attribute `T1`
- scipy/spatial/tests/test_kdtree.py:596:65: error[unresolved-attribute] Object of type `Self@test_large_radius` has no attribute `T2`
- scipy/spatial/tests/test_kdtree.py:596:65: error[unresolved-attribute] Object of type `Self@test_large_radius` has no attribute `T2`
- Found 8771 diagnostics
+ Found 8761 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-24 05:42_

---

_Comment by @github-actions[bot] on 2025-10-24 05:48_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://ibraheem-duplicate-diagnosti.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-duplicate-diagnosti.ecosystem-663.pages.dev/timing))


---

_@MichaReiser approved on 2025-10-24 05:49_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/context.rs`:100 on 2025-10-24 05:50_

Should it be the other way where we don't extend self if other is a multi inference context?

---

_@MichaReiser reviewed on 2025-10-24 05:51_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/context.rs`:100 on 2025-10-24 06:23_

Okay, I think I figured it out. `other` is guaranteed to be empty if it is a `multi_inference` region. That's why we only have to check self

---

_@MichaReiser reviewed on 2025-10-24 06:23_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-24 16:10_

---

_@ibraheemdev reviewed on 2025-10-24 17:21_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/context.rs`:100 on 2025-10-24 17:21_

Right, the issue is that if `other` was created using a different context (e.g. because it was executed as a salsa query), we still want to respect to context of `self`.

---

_Merged by @ibraheemdev on 2025-10-24 17:21_

---

_Closed by @ibraheemdev on 2025-10-24 17:21_

---

_Branch deleted on 2025-10-24 17:21_

---
