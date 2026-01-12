```yaml
number: 20802
title: "[ty] Treat `Callable`s as bound-method descriptors in special cases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/callables-as-descriptors
created_at: 2025-10-10T11:42:42Z
updated_at: 2025-10-14T14:41:58Z
url: https://github.com/astral-sh/ruff/pull/20802
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Treat `Callable`s as bound-method descriptors in special cases

---

_@sharkdp_

## Summary

Treat `Callable`s as bound-method descriptors if `Callable` is the return type of a decorator that is applied to a function definition. See the [rendered version of the new test file](https://github.com/astral-sh/ruff/blob/david/callables-as-descriptors/crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md) for the full description of this new heuristic.

I could imagine that we want to treat `Callable`s as bound-method descriptors in other cases as well, but this seems like a step in the right direction. I am planning to add other "use cases" from https://github.com/astral-sh/ty/issues/491 to this test suite.

partially addresses https://github.com/astral-sh/ty/issues/491
closes https://github.com/astral-sh/ty/issues/1333

## Ecosystem impact

All positive

* 2961 removed `unsupported-operator` diagnostics on `sympy`, which was one of the main motivations for implementing this change
* 37 removed `missing-argument` diagnostics, and no added call-error diagnostics, which is an indicator that this heuristic shouldn't cause many false positives
* A few removed `possibly-missing-attribute` diagnostics when accessing attributes like `__name__` on decorated functions. The two added `unused-ignore-comment` diagnostics are also cases of this.
* One new `invalid-assignment` diagnostic on `dd-trace-py`, which looks suspicious, but only because our `invalid-assignment` diagnostics are not great. This is actually a "Implicit shadowing of function" diagnostic that hides behind the `invalid-assignment` diagnostic, because a module-global function is being patched through a `module.func` attribute assignment.

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-10-10 11:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-10 11:43_

---

_Comment by @github-actions[bot] on 2025-10-10 11:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-10 11:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:759:93: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'unresolved-attribute'
- Found 169 diagnostics
+ Found 170 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_deprecate.py:137:5: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `(...) -> Unknown`.
- src/trio/_deprecate.py:138:5: error[unresolved-attribute] Unresolved attribute `__name__` on type `(...) -> Unknown`.
- Found 647 diagnostics
+ Found 645 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/commands/context.py:181:9: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((...) -> list[str])` may be missing
- pwndbg/gdblib/tui/context.py:216:9: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((...) -> list[str])` may be missing
- Found 2590 diagnostics
+ Found 2588 diagnostics

CPython (cases_generator) (https://github.com/python/cpython)
- Tools/cases_generator/parser.py:69:23: error[missing-argument] No argument provided for required parameter 1
- Tools/cases_generator/parsing.py:738:19: error[missing-argument] No argument provided for required parameter 1
- Found 9 diagnostics
+ Found 7 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/debugging/probe/test_remoteconfig.py:69:9: error[invalid-assignment] Object of type `(...) -> Iterable[Probe]` is not assignable to attribute `get_probes` of type `(...) -> Iterable[Probe]`
- tests/internal/test_packages.py:28:30: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`

sympy (https://github.com/sympy/sympy)
- sympy/algebras/tests/test_quaternion.py:72:37: error[unsupported-operator] Operator `*` is unsupported between objects of type `MutableDenseMatrix` and `MutableDenseMatrix`
- sympy/algebras/tests/test_quaternion.py:73:37: error[unsupported-operator] Operator `*` is unsupported between objects of type `MutableDenseMatrix` and `MutableDenseMatrix`
- sympy/algebras/tests/test_quaternion.py:201:55: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[5]`
- sympy/algebras/tests/test_quaternion.py:201:69: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[5]`
- sympy/algebras/tests/test_quaternion.py:365:66: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[5]`
- sympy/algebras/tests/test_quaternion.py:365:80: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[5]`
- sympy/algebras/tests/test_quaternion.py:420:33: error[unsupported-operator] Operator `-` is unsupported between objects of type `MutableDenseMatrix` and `MutableDenseMatrix`
- sympy/assumptions/tests/test_query.py:2113:32: error[unsupported-operator] Operator `**` is unsupported between objects of type `MutableDenseMatrix` and `Literal[2]`
- sympy/assumptions/tests/test_query.py:2114:10: error[unsupported-operator] Operator `**` is unsupported between objects of type `MutableDenseMatrix` and `Literal[3]`
- sympy/assumptions/tests/test_refine.py:53:26: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `Half`
- sympy/assumptions/tests/test_refine.py:54:26: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | float` and `Half`
- sympy/assumptions/tests/test_refine.py:55:38: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[5]` and `Half`
- sympy/assumptions/tests/test_refine.py:59:38: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[7]` and `Half`
- sympy/assumptions/tests/test_refine.py:60:38: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[9]` and `Half`
- sympy/benchmarks/bench_symbench.py:17:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/benchmarks/bench_symbench.py:99:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[10]`
- sympy/calculus/finite_diff.py:190:30: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `One | Unknown`
- sympy/calculus/tests/test_accumulationbounds.py:25:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AccumulationBounds` and `Literal[1]`
- sympy/calculus/tests/test_accumulationbounds.py:26:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:27:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:31:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `AccumulationBounds` and `Literal[1]`
- sympy/calculus/tests/test_accumulationbounds.py:32:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:33:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:39:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:40:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:41:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:43:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:45:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:47:13: error[unsupported-operator] Operator `-` is unsupported between objects of type `Infinity` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:50:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:51:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[2]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:52:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:66:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:67:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:68:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:69:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:71:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:72:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:74:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:75:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:76:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:77:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:78:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:79:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:83:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:84:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:87:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:89:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `Infinity`
- sympy/calculus/tests/test_accumulationbounds.py:93:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:94:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:95:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:99:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:100:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:102:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:103:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:104:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:106:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:107:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:108:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:110:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:111:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:112:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[-1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:113:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:114:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:115:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[-2]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:116:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:120:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:140:25: error[unsupported-operator] Operator `*` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:141:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:154:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:160:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:161:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:162:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:163:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[3]`
- sympy/calculus/tests/test_accumulationbounds.py:164:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[0]`
- sympy/calculus/tests/test_accumulationbounds.py:181:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:182:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:183:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-3]`
- sympy/calculus/tests/test_accumulationbounds.py:184:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-3]`
- sympy/calculus/tests/test_accumulationbounds.py:185:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:186:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-3]`
- sympy/calculus/tests/test_accumulationbounds.py:187:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-3]`
- sympy/calculus/tests/test_accumulationbounds.py:188:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:190:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:191:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `Literal[-2]`
- sympy/calculus/tests/test_accumulationbounds.py:234:34: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/calculus/tests/test_accumulationbounds.py:235:29: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/calculus/tests/test_accumulationbounds.py:239:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:239:23: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:240:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:243:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:243:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[27]`
- sympy/calculus/tests/test_accumulationbounds.py:243:45: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/calculus/tests/test_accumulationbounds.py:244:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:244:35: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[27]`
- sympy/calculus/tests/test_accumulationbounds.py:248:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:248:17: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:248:43: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:249:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:250:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:251:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:252:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:252:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:253:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:253:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:254:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:254:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:255:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:255:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:256:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:256:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:257:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:257:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:258:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:258:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:259:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:259:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:260:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:260:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:261:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:262:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:263:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:264:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:265:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:266:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:267:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:268:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_accumulationbounds.py:268:41: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_accumulationbounds.py:269:12: error[unsupported-operator] Operator `**` is unsupported between objects of type `AccumulationBounds` and `AccumulationBounds`
- sympy/calculus/tests/test_finite_diff.py:81:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `int` and `Integer`
- sympy/calculus/tests/test_util.py:184:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/calculus/tests/test_util.py:403:40: error[unsupported-operator] Operator `/` is unsupported between objects of type `Half` and `Literal[2]`
- sympy/calculus/tests/test_util.py:407:60: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/codegen/tests/test_algorithms.py:177:19: error[unsupported-operator] Operator `*` is unsupported between objects of type `Float` and `Unknown | float`
- sympy/codegen/tests/test_ast.py:536:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `Float` and `complex`
- sympy/codegen/tests/test_cfunctions.py:139:45: error[unsupported-operator] Operator `-` is unsupported between objects of type `Rational` and `Literal[1]`
- sympy/codegen/tests/test_cfunctions.py:150:45: error[unsupported-operator] Operator `-` is unsupported between objects of type `Half` and `Literal[1]`
- sympy/codegen/tests/test_matrix_nodes.py:26:37: error[unsupported-operator] Operator `*` is unsupported between objects of type `MutableDenseMatrix` and `MutableDenseMatrix`
- sympy/codegen/tests/test_matrix_nodes.py:27:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `MutableDenseMatrix` and `MutableDenseMatrix`
- sympy/codegen/tests/test_rewriting.py:430:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[58]` and `Integer`
- sympy/codegen/tests/test_rewriting.py:430:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[97]` and `Integer`
- sympy/codegen/tests/test_rewriting.py:430:51: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[4]` and `Integer`
- sympy/codegen/tests/test_rewriting.py:430:64: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[92]` and `Integer`
- sympy/concrete/tests/test_sums_products.py:346:52: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:572:42: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[10]`
- sympy/concrete/tests/test_sums_products.py:575:40: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[10]`
- sympy/concrete/tests/test_sums_products.py:1183:35: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/concrete/tests/test_sums_products.py:1569:44: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/concrete/tests/test_sums_products.py:1571:39: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1579:9: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1587:27: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1594:70: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1595:70: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1596:69: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1597:69: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1598:82: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/concrete/tests/test_sums_products.py:1599:79: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/add.py:269:25: error[missing-argument] No argument provided for required parameter 2
- sympy/core/add.py:321:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Number` and `Number`
- sympy/core/benchmarks/bench_numbers.py:88:5: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Literal[1]`
- sympy/core/benchmarks/bench_numbers.py:92:5: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/evalf.py:1224:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | NegativeInfinity` and `int | NegativeInfinity`
- sympy/core/evalf.py:1278:13: error[unsupported-operator] Operator `*=` is unsupported between objects of type `Rational` and `int | Unknown`
- sympy/core/expr.py:2625:30: error[unsupported-operator] Operator `/` is unsupported between objects of type `Zero | Unknown` and `Literal[2]`
- sympy/core/expr.py:4091:34: error[unsupported-operator] Operator `/` is unsupported between objects of type `Float` and `int | float`
- sympy/core/mul.py:1920:25: error[unsupported-operator] Operator `-=` is unsupported between objects of type `Infinity & ~AlwaysFalsy` and `(Unknown & ~AlwaysFalsy) | Literal[1]`
- sympy/core/numbers.py:378:24: error[unsupported-operator] Operator `*` is unsupported between objects of type `Number & ~AlwaysFalsy` and `int`
- sympy/core/random.py:127:10: error[missing-argument] No argument provided for required parameter 2
- sympy/core/tests/test_args.py:5425:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `DyadicAdd`
- sympy/core/tests/test_arit.py:119:29: error[unsupported-operator] Operator `/` is unsupported between objects of type `Rational` and `Literal[2]`
- sympy/core/tests/test_arit.py:149:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_arit.py:153:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_arit.py:213:51: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_arit.py:213:62: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[6]`
- sympy/core/tests/test_arit.py:213:70: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[32]`
- sympy/core/tests/test_arit.py:1856:14: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `Literal[1]`
- sympy/core/tests/test_arit.py:1918:17: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[10]`
- sympy/core/tests/test_arit.py:1918:42: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[10]`
- sympy/core/tests/test_arit.py:2218:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[1]` and `Rational`
- sympy/core/tests/test_arit.py:2487:24: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_arit.py:2487:46: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_assumptions.py:1030:33: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[0]` and `Infinity`
- sympy/core/tests/test_assumptions.py:1031:8: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[0]` and `Infinity`
- sympy/core/tests/test_complex.py:148:10: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_complex.py:150:10: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_complex.py:151:26: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[1024]`
- sympy/core/tests/test_complex.py:205:28: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_count_ops.py:57:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/core/tests/test_count_ops.py:81:22: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_eval.py:21:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_evalf.py:273:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `int`
- sympy/core/tests/test_expand.py:150:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_expr.py:441:35: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | Rational | Float | Symbol` and `Literal["1"]`
- sympy/core/tests/test_expr.py:442:35: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | Rational | Float | Symbol` and `Literal["1"]`
- sympy/core/tests/test_expr.py:444:20: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | Rational | Float | Symbol` and `Literal["1"]`
- sympy/core/tests/test_expr.py:446:39: error[unsupported-operator] Operator `*` is unsupported between objects of type `(Unknown & ~Literal[2]) | Rational | Float | Symbol` and `Literal["1"]`
- sympy/core/tests/test_expr.py:447:35: error[unsupported-operator] Operator `/` is unsupported between objects of type `Unknown | Rational | Float | Symbol` and `Literal["1"]`
- sympy/core/tests/test_expr.py:669:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `NaN | Infinity | NegativeInfinity | ComplexInfinity` and `Literal[1] | Symbol`
- sympy/core/tests/test_expr.py:705:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:709:27: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/core/tests/test_expr.py:710:27: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/core/tests/test_expr.py:711:27: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[3]`
- sympy/core/tests/test_expr.py:725:9: error[unsupported-operator] Operator `*` is unsupported between objects of type `Rational` and `MyInt`
- sympy/core/tests/test_expr.py:728:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Rational` and `<class 'MyInt'>`
- sympy/core/tests/test_expr.py:736:9: error[unsupported-operator] Operator `*` is unsupported between objects of type `Rational` and `MyInt`
- sympy/core/tests/test_expr.py:739:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Rational` and `<class 'MyInt'>`
- sympy/core/tests/test_expr.py:1446:38: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[8]`
- sympy/core/tests/test_expr.py:1878:10: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[14]`
- sympy/core/tests/test_expr.py:1881:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1883:10: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[21]`
- sympy/core/tests/test_expr.py:1940:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_expr.py:1940:30: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:1940:48: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:1947:23: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_expr.py:1947:47: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_expr.py:1948:23: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_expr.py:1948:47: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_expr.py:1952:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `Rational` and `Literal[2]`
- sympy/core/tests/test_expr.py:1970:29: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1970:47: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1970:64: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1971:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1971:31: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_expr.py:1971:54: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1972:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1972:22: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1972:33: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1972:54: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1973:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1973:22: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1973:33: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[6]`
- sympy/core/tests/test_expr.py:1973:46: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:1974:7: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1974:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1974:42: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1974:53: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1975:7: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_expr.py:1975:30: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1975:48: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1975:65: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1976:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1976:26: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1976:44: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1976:61: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1977:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[6]`
- sympy/core/tests/test_expr.py:1977:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:1977:49: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1978:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1978:22: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1978:33: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1979:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_expr.py:1979:28: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1979:46: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1979:63: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1980:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1980:26: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1980:44: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1980:61: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1981:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[6]`
- sympy/core/tests/test_expr.py:1981:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:1981:51: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1982:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1982:22: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1982:33: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1983:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[4]`
- sympy/core/tests/test_expr.py:1983:28: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1983:46: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1983:63: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1984:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[12]`
- sympy/core/tests/test_expr.py:1984:26: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[8]`
- sympy/core/tests/test_expr.py:1984:44: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[13824]`
- sympy/core/tests/test_expr.py:1984:61: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_expr.py:1985:5: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[6]`
- sympy/core/tests/test_expr.py:1985:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_expr.py:2142:20: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `Integer`
- sympy/core/tests/test_expr.py:2208:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Float` and `Literal[1]`
- sympy/core/tests/test_exprtools.py:73:46: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_exprtools.py:73:70: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_exprtools.py:74:24: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_exprtools.py:74:50: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_exprtools.py:165:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[4]`
- sympy/core/tests/test_exprtools.py:177:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[64]`
- sympy/core/tests/test_exprtools.py:179:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[8]`
- sympy/core/tests/test_exprtools.py:284:50: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_exprtools.py:284:71: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_function.py:563:83: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_function.py:564:87: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[2]`
- sympy/core/tests/test_function.py:628:16: error[missing-argument] No argument provided for required parameter 2
- sympy/core/tests/test_function.py:921:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_function.py:922:65: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_function.py:1075:14: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[12]`
- sympy/core/tests/test_function.py:1097:45: error[unsupported-operator] Operator `/` is unsupported between objects of type `Integer` and `Literal[24]`
- sympy/core/tests/test_function.py:1099:26: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[24]`
- sympy/core/tests/test_function.py:1133:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[8]`
- sympy/core/tests/test_match.py:32:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Literal[1]`
- sympy/core/tests/test_match.py:396:34: error[unsupported-operator] Operator `/` is unsupported between objects of type `Rational` and `Literal[3]`
- sympy/core/tests/test_match.py:600:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_match.py:605:36: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[3]`
- sympy/core/tests/test_match.py:631:32: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `Integer`
- sympy/core/tests/test_match.py:657:13: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[8]`
- sympy/core/tests/test_match.py:658:24: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[8]`
- sympy/core/tests/test_numbers.py:54:32: error[unsupported-operator] Operator `/` is unsupported between objects of type `Zero` and `Zero`
- sympy/core/tests/test_numbers.py:56:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Zero` and `Zero`
- sympy/core/tests/test_numbers.py:64:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Half` and `Half`
- sympy/core/tests/test_numbers.py:65:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Half` and `Rational`
- sympy/core/tests/test_numbers.py:66:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Half` and `Rational`
- sympy/core/tests/test_numbers.py:67:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Half`
- sympy/core/tests/test_numbers.py:68:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:69:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:70:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Half`
- sympy/core/tests/test_numbers.py:71:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:72:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:76:13: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:77:13: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `Literal[2]`
- sympy/core/tests/test_numbers.py:78:13: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:82:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:84:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Literal[5]` and `Infinity`
- sympy/core/tests/test_numbers.py:93:9: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Float`
- sympy/core/tests/test_numbers.py:94:17: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:97:9: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `Rational`
- sympy/core/tests/test_numbers.py:98:17: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:107:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Rational` and `Float`
- sympy/core/tests/test_numbers.py:108:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Float` and `Rational`
- sympy/core/tests/test_numbers.py:109:12: error[missing-argument] No argument provided for required parameter 2
- sympy/core/tests/test_numbers.py:110:12: error[missing-argument] No argument provided for required parameter 2
- sympy/core/tests/test_numbers.py:111:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `float` and `Float`
- sympy/core/tests/test_numbers.py:442:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Literal[1]`
- sympy/core/tests/test_numbers.py:443:14: error[unsupported-operator] Operator `+` is unsupported between objects of type `Rational` and `Rational`
- sympy/core/tests/test_numbers.py:620:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[10]`
- sympy/core/tests/test_numbers.py:621:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[10]`
- sympy/core/tests/test_numbers.py:622:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[10]`
- sympy/core/tests/test_numbers.py:623:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[10]`
- sympy/core/tests/test_numbers.py:687:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Float` and `Float`
- sympy/core/tests/test_numbers.py:692:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Float` and `Float`
- sympy/core/tests/test_numbers.py:715:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[1]` and `Infinity`
- sympy/core/tests/test_numbers.py:719:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `Literal[1]`
- sympy/core/tests/test_numbers.py:720:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[2]` and `Infinity`
- sympy/core/tests/test_numbers.py:721:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[3]` and `Infinity`
- sympy/core/tests/test_numbers.py:725:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:726:18: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `Literal[-5]`
- sympy/core/tests/test_numbers.py:730:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Infinity` and `Literal[2]`
- sympy/core/tests/test_numbers.py:731:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `Literal[2]` and `Infinity`
- sympy/core/tests/test_numbers.py:732:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:736:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:742:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:746:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `Infinity`
- sympy/core/tests/test_numbers.py:750:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `Literal[0]`
- sympy/core/tests/test_numbers.py:754:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `Literal[0]`
- sympy/core/tests/test_numbers.py:756:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[0]` and `Infinity`
- sympy/core/tests/test_numbers.py:758:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `Literal[0]`
- sympy/core/tests/test_numbers.py:760:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[0]` and `Infinity`
- sympy/core/tests/test_numbers.py:762:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Infinity` and `Literal[0]`
- sympy/core/tests/test_numbers.py:764:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[0]` and `Infinity`
- sympy/core/tests/test_numbers.py:766:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `Literal[2]`
- sympy/core/tests/test_numbers.py:768:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `Literal[-2]`
- sympy/core/tests/test_numbers.py:770:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `Literal[2]`
- sympy/core/tests/test_numbers.py:772:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `Literal[-2]`
- sympy/core/tests/test_numbers.py:777:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `Infinity`
- sympy/core/tests/test_numbers.py:779:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[-2]` and `Infinity`
- sympy/core/tests/test_numbers.py:781:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[2]` and `Infinity`
- sympy/core/tests/test_numbers.py:782:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[2]` and `Infinity`
- sympy/core/tests/test_numbers.py:783:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[-2]` and `Infinity`
- sympy/core/tests/test_numbers.py:784:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[-2]` and `Infinity`
- sympy/core/tests/test_numbers.py:793:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:793:37: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:795:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:795:37: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:797:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:797:39: error[unsupported-operator] Operator `*` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:799:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:799:39: error[unsupported-operator] Operator `/` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:801:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:801:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:803:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:803:39: error[unsupported-operator] Operator `-` is unsupported between objects of type `Infinity` and `float`
- sympy/core/tests/test_numbers.py:805:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:805:37: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:809:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:809:39: error[unsupported-operator] Operator `*` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:813:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:815:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `float` and `Infinity`
- sympy/core/tests/test_numbers.py:825:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `NaN` and `float`
- sympy/core/tests/test_numbers.py:827:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `NaN` and `Infinity`
- sympy/core/tests/test_numbers.py:831:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `NaN` and `Infinity`
- sympy/core/tests/test_numbers.py:833:12: error[unsupported-operator] Operator `-` is unsupported between objects of type `NaN` and `Infinity`
- sympy/core/tests/test_numbers.py:862:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `NaN` and `One`
- sympy/core/tests/test_numbers.py:898:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:899:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:900:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:901:12: error[unsupported-operator] Operator `*` is unsupported between objects of type `Float` and `float`
- sympy/core/tests/test_numbers.py:909:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `Zero`
- sympy/core/tests/test_numbers.py:911:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[0]` and `Zero`
- sympy/core/tests/test_numbers.py:913:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Zero` and `Literal[0]`
- sympy/core/tests/test_numbers.py:914:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Float` and `Literal[0]`
- sympy/core/tests/test_numbers.py:915:12: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[-1]` and `Zero`
- sympy/core/tests/test_numbers.py:983:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `NaN` and `NaN`
- sympy/core/tests/test_numbers.py:984:19: error[unsupported-operator] Operator `*` is unsupported between objects of type `NaN` and `Literal[-5]`
- sympy/core/tests/test_numbers.py:1108:18: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[4503599761588223]` and `Half`
- sympy/core/tests/test_numbers.py:1109:18: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[4503599761588224]` and `Half`
- sympy/core/tests/test_numbers.py:1110:18: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[4503599761588225]` and `Half`
- sympy/core/tests/test_numbers.py:1123:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[1]` and `Rational`
- sympy/core/tests/test_numbers.py:1124:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[4503599761588224]` and `Rational`
- sympy/core/tests/test_numbers.py:1125:18: error[unsupported-operator] Operator `-` is unsupported between objects of type `int` and `Rational`
- sympy/core/tests/test_numbers.py:1142:56: error[unsupported-operator] Operator `/` is unsupported between objects of type `One` and `Literal[5]`
- sympy/core/tests/test_numbers.py:1500:17: error[unsupported-...*[Comment body truncated]*

---

_Renamed from "[ty] Treat `Callable`s as bound-method descripors in special cases" to "[ty] Treat `Callable`s as bound-method descriptors in special cases" by @sharkdp on 2025-10-10 11:46_

---

_Comment by @github-actions[bot] on 2025-10-10 11:48_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 2,961 | 0 |
| `missing-argument` | 0 | 37 | 0 |
| `unresolved-attribute` | 0 | 3 | 0 |
| `possibly-missing-attribute` | 0 | 2 | 0 |
| `unused-ignore-comment` | 2 | 0 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| **Total** | **3** | **3,004** | **0** |

**[Full report with detailed diff](https://david-callables-as-descripto.ecosystem-663.pages.dev/diff)** ([timing results](https://david-callables-as-descripto.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-10 12:09_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-10 12:09_

---

_@sharkdp reviewed on 2025-10-13 09:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:8 on 2025-10-13 09:42_

Part of the text here is a copy from @carljm's post here: https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938

---

_@sharkdp reviewed on 2025-10-13 09:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:590 on 2025-10-13 09:46_

This is the fix to https://github.com/astral-sh/ty/issues/1333

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-13 13:22_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-13 13:22_

---

_@sharkdp reviewed on 2025-10-13 13:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:190 on 2025-10-13 13:45_

The argument here is a bit weak. I had trouble coming up with a better example.

It still feels wrong to me to generally assume that returned `Callable` types are function-like if a function-like callable is passed into a call (would we try to detect that in every possible parameter position? or only apply the heuristic for single-argument calls?).

---

_@sharkdp reviewed on 2025-10-13 13:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1017 on 2025-10-13 13:48_

According to our usual naming logic, this function should be called `into_callable`. But that function already exists (and does something else, namely try to *turn* a `Type` into its equivalent `Callable` type). I would propose to rename the existing `Type::into_callable` in order to restore our naming scheme here.

As an aside, I always try `ty.as_*` first (e.g. `ty.as_class_literal()`) before remembering that they all start with `.into_`. "into" sounds like a transformation, but it's really just an unwrapping.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-13 13:48_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-13 13:48_

---

_Marked ready for review by @sharkdp on 2025-10-13 13:49_

---

_Review requested from @carljm by @sharkdp on 2025-10-13 13:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-13 13:49_

---

_Review requested from @dcreager by @sharkdp on 2025-10-13 13:49_

---

_@AlexWaygood reviewed on 2025-10-13 13:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1017 on 2025-10-13 13:55_

IIRC Clippy complains if you name it with `as_*`, because the convention is that `as_*` funcitons are meant to take `&self`, whereas our various `into_*` functions consume `self`? I might be misremembering, though

---

_@AlexWaygood reviewed on 2025-10-13 14:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:588 on 2025-10-13 14:53_

shouldn't positional-or-keyword parameters be treated the same way as positional-only parameters here?

Also: for something like this:

```py
class F:
    def method(*args: *tuple[F, int, *tuple[str, ...]]): ...
```

the signature of `F().method` should arguably be `(*args: *tuple[int, *tuple[str, ...]]) -> Unknown`: `bind_self` should pop the first element off the mixed tuple's prefix.

We don't need to worry about such edge cases now, but we could add a TODO comment possibly.

---

_@AlexWaygood reviewed on 2025-10-13 14:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2181 on 2025-10-13 14:53_

```suggestion
                        .and_then(Type::unwrap_as_callable_type)
```

---

_@sharkdp reviewed on 2025-10-13 14:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2181 on 2025-10-13 14:55_

huh, weird. I made that change locally, but apparently the change didn't make it into the final commit.

---

_@AlexWaygood reviewed on 2025-10-13 14:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:588 on 2025-10-13 14:57_

(edited the above snippet because I forgot that it needs to be `*args: *tuple[...]` unless you want to say "every element of `*args` needs to have the fancy tuple type")

---

_@sharkdp reviewed on 2025-10-13 14:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:588 on 2025-10-13 14:57_

> shouldn't positional-or-keyword parameters be treated the same way as positional-only parameters here?

`is_positional` includes positional-or-keyword parameters as well



> We don't need to worry about such edge cases now, but we could add a TODO comment possibly.

will do

---

_@AlexWaygood reviewed on 2025-10-13 15:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:588 on 2025-10-13 15:06_

> `is_positional` includes positional-or-keyword parameters as well

argh, sorry. I looked it up to check it, opened `signatures.rs`, did CMD+F for `is_positional`, jumped to the definition for `is_positional_only` (because it was above the definition for `is_positional`) and didn't notice that I was reading the wrong method definition...

---

_@sharkdp reviewed on 2025-10-13 16:04_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:588 on 2025-10-13 16:04_

No worries. I also checked again because I wasn't sure :smile: 

---

_@AlexWaygood reviewed on 2025-10-13 16:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:584 on 2025-10-13 16:08_

Same mistake [I originally made](https://github.com/astral-sh/ruff/pull/20802#discussion_r2426595738) 😄

This needs to be either a starred tuple or `Unpack[tuple[MyClass, int, *tuple[str, ...]]]`. `*args: tuple[int, ...]` means "in the method body, `*args` will be a tuple of unknown length in which each element of the tuple has type `tuple[int, ...]`". `*args: *tuple[int, ...]` means "in the method body, the variable `args` will be a tuple of unknown length in which each element has type `int`"

```suggestion
        // TODO: Theoretically, for a signature like `f(*args: *tuple[MyClass, int, *tuple[str, ...]])` with
```

---

_@AlexWaygood reviewed on 2025-10-13 16:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:585 on 2025-10-13 16:08_

```suggestion
        // a variadic first parameter, we should also "skip the first parameter" by modifying the tuple
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:10 on 2025-10-13 16:10_

`lambda` expressions create instances of `types.FunctionType` just like functions defined with `def` statements, so arguably there's no difference here

```suggestion
Some common callable objects (most commonly, functions!) are also bound-method descriptors. That is, they
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:28 on 2025-10-13 16:13_

off-topic: a `reveal_typeform` function would be pretty useful for this kind of mdtest (and for playing around in the playground). Rather than having to create the function, I could just do `reveal_typeform(CallableTypeOf[C1.method])` and it would print the signature in a diagnostic

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:33 on 2025-10-13 16:18_

`staticmethod` _does_ actually have a `__get__` method at runtime -- it returns the underlying function object (whether it's accessed on the class or the instance). On Python <=3.9, `staticmethod`s are not directly callable; it's only by calling `__get__` on the staticmethod that you retrieve the original function object and get something that you can actually call:

```pycon
% uvx python3.9                             
Python 3.9.6 (default, Apr 30 2025, 02:07:17) 
[Clang 17.0.0 (clang-1700.0.13.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo:
...     @staticmethod
...     def bar(): return 42
... 
>>> original_staticmethod = Foo.__dict__['bar']
>>> original_staticmethod
<staticmethod object at 0x100575370>
>>> original_staticmethod()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'staticmethod' object is not callable
>>> Foo.bar
<function Foo.bar at 0x1005fb4c0>
>>> Foo.bar()
42
```

On Python 3.10, `staticmethod.__call__` was added, so `staticmethod` objects are now directly callable. But it's still the case that staticmethods are descriptors that return the underlying function object when you access them, on either the class object or an instance of the class.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:190 on 2025-10-13 16:28_

The most common example where a non-function callable is returned by a decorator is `functools._lru_cache_wrapper`:

```pycon
% py
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import functools
>>> class F:
...     @functools.lru_cache
...     def whatever(self): ...
...     
>>> F.whatever
<functools._lru_cache_wrapper object at 0x101549640>
```

But ([somewhat](https://github.com/python/typeshed/issues/11280) [famously](https://github.com/python/typeshed/issues/6347)), `_lru_cache_wrapper` has the same descriptor-binding behaviour that `types.FunctionType` has. So you could use that as an argument in favour of assuming that all objects returned from decorators have this method-binding behaviour if they have `__call__` attributes, I suppose...

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1017 on 2025-10-13 16:29_

> I would propose to rename the existing `Type::into_callable` in order to restore our naming scheme here.

That makes sense to me. Maybe `try_upcast_to_callable`?

---

_@AlexWaygood reviewed on 2025-10-13 16:32_

Nearly all the ecosystem diff appears to be due to the `bind_self` fix in `signatures.rs`, which actually seems unrelated to the rest of the PR... I wonder if we could separate that out into a standalone PR, so that we can see the impact of the special case here more clearly?

---

_@sharkdp reviewed on 2025-10-13 18:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/signatures.rs`:584 on 2025-10-13 18:16_

Oh, I didn't see your edit.

---

_@sharkdp reviewed on 2025-10-13 18:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:10 on 2025-10-13 18:31_

Right, I still found it helpful for them to be listed here (this is from Carl's post). They might have a common supertype in `FunctionType`, but they are created by completely different syntactical constructs.

---

_@AlexWaygood reviewed on 2025-10-13 18:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:10 on 2025-10-13 18:34_

Maybe something like this?

```suggestion
Some common callable objects (all functions, including lambdas) are also bound-method descriptors. That is, they
```

---

_@sharkdp reviewed on 2025-10-13 18:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:28 on 2025-10-13 18:42_

> a reveal_typeform function would be pretty useful

Yes!!

---

_@AlexWaygood approved on 2025-10-13 18:48_

---

_@sharkdp reviewed on 2025-10-13 18:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:33 on 2025-10-13 18:50_

> `staticmethod` _does_ actually have a `__get__` method at runtime -- it returns the underlying function object

Right, I know. The text says "Other callable objects (`staticmethod` objects, […]) are *not* bound-method descriptors", where "bound-method descriptor" would imply the sort of `self`-binding behavior that we know from normal functions. But the text could certainly be clarified further, if you think that'd be valuable.

> On Python 3.10, `staticmethod.__call__` was added, so `staticmethod` objects are now directly callable.

TIL!

---

_@AlexWaygood reviewed on 2025-10-13 18:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:33 on 2025-10-13 18:54_

> The text says "Other callable objects (`staticmethod` objects, […]) are _not_ bound-method descriptors"

Hmmmm, but it also says:

> If accessed as class attributes via
an instance, they are simply themselves

Which I don't think is accurate for staticmethods — they _are_ descriptors, just not in the same way as instance methods

---

_@sharkdp reviewed on 2025-10-13 18:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/callables_as_descriptors.md`:28 on 2025-10-13 18:58_

Or if Python had explicitly specialized calls to generic functions, we could *finally* add C++'s [`std::declval`](https://en.cppreference.com/w/cpp/utility/declval.html):
```py
def declval[T]() -> T:
    raise RuntimeError("can not make up a `T` out of thin air")

reveal_type(declval[CallableTypeOf[C1.method]]())
```

Beautiful!

---

_@sharkdp reviewed on 2025-10-13 19:00_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1017 on 2025-10-13 19:00_

Will do that as a follow-up

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-13 19:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-13 19:01_

---

_@AlexWaygood reviewed on 2025-10-13 19:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2181 on 2025-10-13 19:03_

Note that `unwrap_as_callable_type` will return `None` if `into_callable` returns `Some(Type::Union)`, even if each element of the union returned was a `Callable` type

---

_@sharkdp reviewed on 2025-10-13 19:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:2181 on 2025-10-13 19:07_

Oh, that's a good catch! Will look into this tomorrow (post-merge, I don't think it's critical).

---

_Merged by @sharkdp on 2025-10-13 19:17_

---

_Closed by @sharkdp on 2025-10-13 19:17_

---

_Branch deleted on 2025-10-13 19:17_

---

_Comment by @AlexWaygood on 2025-10-14 14:41_

It looks like this PR might have caused the ty fuzzer job to take significantly longer in CI. In PRs yesterday, it took around 5 minutes to complete, but following this PR it seems to be taking around 18 minutes to complete. It looks like this change causes pathological performance issues on seeds 32 and 56, for some reason.

It doesn't seem to be causing issues on any of our benchmarks, and mypy_primer still seems to be finishing in good time, so the problematic pattern is probably something that doesn't actually come up much in practice? So a quick fix might be to just add 32 and 56 to this set here: https://github.com/astral-sh/ruff/blob/1ed9b215b981d0283022185f988957d22a02b918/python/py-fuzzer/fuzz.py#L155-L156

We should probably try to pin down what exactly the problematic pattern _is_, though, in the medium/long term

---
