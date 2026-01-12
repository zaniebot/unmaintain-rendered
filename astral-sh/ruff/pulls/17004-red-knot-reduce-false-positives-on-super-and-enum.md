```yaml
number: 17004
title: "[red-knot] Reduce false positives on `super()` and enum-class attribute accesses"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/enum-attrs
created_at: 2025-03-26T20:38:59Z
updated_at: 2025-03-27T21:30:58Z
url: https://github.com/astral-sh/ruff/pull/17004
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Reduce false positives on `super()` and enum-class attribute accesses

---

_@AlexWaygood_

## Summary

This PR adds some branches so that we infer `Todo` types for attribute access on instances of `super()` and subtypes of `type[Enum]`; reduces false positives in the short term until we implement full support for these features.

## Test Plan

New mdtests added + mypy_primer report


---

_Label `red-knot` added by @AlexWaygood on 2025-03-26 20:39_

---

_Comment by @github-actions[bot] on 2025-03-26 20:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:465:15: Type `super` has no attribute `release`
- Found 123 diagnostics
+ Found 122 diagnostics

zipp (https://github.com/jaraco/zipp)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:122:17: Type `super` has no attribute `namelist`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:143:20: Type `super` has no attribute `getinfo`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:190:16: Type `super` has no attribute `namelist`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:197:16: Type `super` has no attribute `_name_set`
- Found 9 diagnostics
+ Found 5 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:398:21: Type `super` has no attribute `describe`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:1256:16: Type `super` has no attribute `_format_relative`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:2123:20: Type `super` has no attribute `describe`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:5651:20: Type `super` has no attribute `describe`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:6111:20: Type `super` has no attribute `describe`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/locales.py:6484:20: Type `super` has no attribute `describe`
- Found 52 diagnostics
+ Found 46 diagnostics

git-revise (https://github.com/mystor/git-revise)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:25:20: Object of type `Unknown | Literal["pick"]` is not assignable to return type `StepKind`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:27:20: Object of type `Unknown | Literal["fixup"]` is not assignable to return type `StepKind`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:29:20: Object of type `Unknown | Literal["squash"]` is not assignable to return type `StepKind`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:31:20: Object of type `Unknown | Literal["reword"]` is not assignable to return type `StepKind`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:33:20: Object of type `Unknown | Literal["cut"]` is not assignable to return type `StepKind`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:35:20: Object of type `Unknown | Literal["index"]` is not assignable to return type `StepKind`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/git-revise/gitrevise/odb.py:60:41: Unused blanket `type: ignore` directive
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/git-revise/gitrevise/odb.py:491:32: Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 24 diagnostics
+ Found 18 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/url_safe.py:53:16: Type `super` has no attribute `load_payload`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/url_safe.py:56:16: Type `super` has no attribute `dump_payload`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/timed.py:89:22: Type `super` has no attribute `unsign`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/timed.py:180:57: Type `super` has no attribute `iter_unsigners`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:105:28: Type `super` has no attribute `unsign`
- Found 35 diagnostics
+ Found 30 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/isort/isort/output.py:654:36: Too many positional arguments to bound method `__new__`: expected 1, got 3
- Found 54 diagnostics
+ Found 53 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/highlighter.py:124:9: Type `super` has no attribute `highlight`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/tests/test_style.py:57:40: Object of type `Unknown | Literal[3]` cannot be assigned to parameter 2 (`color_system`) of bound method `_make_ansi_codes`; expected type `ColorSystem`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/color.py:336:20: Object of type `Unknown | Literal[1]` is not assignable to return type `ColorSystem`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:794:20: Object of type `Unknown | Literal[3]` is not assignable to return type `ColorSystem | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:799:24: Object of type `Unknown | Literal[4]` is not assignable to return type `ColorSystem | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:802:17: Object of type `Unknown | Literal[3, 2]` is not assignable to return type `ColorSystem | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:809:24: Object of type `Unknown | Literal[3]` is not assignable to return type `ColorSystem | None`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/rich/rich/style.py:698:9: Default value of type `Unknown | Literal[3]` is not assignable to annotated parameter type `ColorSystem | None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/markdown.py:442:9: Type `super` has no attribute `on_enter`
- Found 585 diagnostics
+ Found 576 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_thread.py:25:9: Type `super` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:287:9: Type `super` has no attribute `build_extensions`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:21:20: Type `super` has no attribute `run`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:255:17: Type `super` has no attribute `dispatch`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/setup.py:100:9: Type `super` has no attribute `make_release_tree`
- Found 275 diagnostics
+ Found 270 diagnostics

black (https://github.com/psf/black)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/lines.py:815:8: Operator `not in` is not supported for types `auto` and `Mode`, in comparing `Unknown | auto` with `Mode`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/comments.py:326:12: Operator `not in` is not supported for types `auto` and `Mode`, in comparing `Unknown | auto` with `Mode`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/parsing.py:39:26: Object of type `Unknown | Literal[6]` cannot be assigned to parameter 2 (`feature`) of function `supports_feature`; expected type `Feature`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/parsing.py:40:49: Object of type `Unknown | Literal[11]` cannot be assigned to parameter 2 (`feature`) of function `supports_feature`; expected type `Feature`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/parsing.py:43:46: Object of type `Unknown | Literal[7]` cannot be assigned to parameter 2 (`feature`) of function `supports_feature`; expected type `Feature`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:142:30: Object of type `Unknown | Literal[1]` cannot be assigned to parameter 3 (`changed`) of bound method `done`; expected type `Changed`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:187:34: Object of type `Unknown | Literal[2, 0]` cannot be assigned to parameter 3 (`changed`) of bound method `done`; expected type `Changed`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:860:27: Object of type `Unknown | Literal[0, 2]` cannot be assigned to parameter 3 (`changed`) of bound method `done`; expected type `Changed`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:919:26: Object of type `Unknown | Literal[0, 2, 1]` cannot be assigned to parameter 3 (`changed`) of bound method `done`; expected type `Changed`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/black/src/black/__init__.py:930:5: Default value of type `Unknown | Literal[0]` is not assignable to annotated parameter type `WriteBack`
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/black/src/black/__init__.py:998:5: Default value of type `Unknown | Literal[0]` is not assignable to annotated parameter type `WriteBack`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/linegen.py:161:20: Type `super` has no attribute `visit_default`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/linegen.py:653:12: Operator `in` is not supported for types `auto` and `Mode`, in comparing `Unknown | auto` with `Mode`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/linegen.py:692:12: Operator `in` is not supported for types `auto` and `Mode`, in comparing `Unknown | auto` with `Mode`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:809:46: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:812:46: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:815:46: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/linegen.py:881:9: Operator `in` is not supported for types `auto` and `Mode`, in comparing `Unknown | auto` with `Mode`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:907:17: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:933:45: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:937:49: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/linegen.py:940:45: Object of type `Unknown | auto` cannot be assigned to parameter `component` of function `bracket_split_build_line`; expected type `_BracketSplitComponent`
- Found 233 diagnostics
+ Found 211 diagnostics

```
</details>


---

_Marked ready for review by @AlexWaygood on 2025-03-26 20:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-26 20:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-26 20:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-26 20:53_

---

_@carljm approved on 2025-03-27 21:29_

Thanks!

---

_Merged by @AlexWaygood on 2025-03-27 21:30_

---

_Closed by @AlexWaygood on 2025-03-27 21:30_

---

_Branch deleted on 2025-03-27 21:30_

---
