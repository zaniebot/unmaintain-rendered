```yaml
number: 17095
title: "[red-knot] Add assignability of function literals to callables"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: funcion-literal-assignability
created_at: 2025-03-31T14:57:01Z
updated_at: 2025-03-31T15:43:01Z
url: https://github.com/astral-sh/ruff/pull/17095
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add assignability of function literals to callables

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Part  of #16953

## Test Plan

Update is_assignable_to.md


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-31 14:57_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-31 14:57_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-31 14:57_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-31 14:57_

---

_Comment by @github-actions[bot] on 2025-03-31 15:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pyinstrument/test/util.py:122:12: Object of type `Literal[wrapped]` is not assignable to return type `() -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_context.py:48:19: Object of type `Literal[busy_wait]` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_context.py:49:19: Object of type `Literal[busy_wait]` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- Found 273 diagnostics
+ Found 270 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/isort/isort/literal.py:84:12: Object of type `Literal[wrap]` is not assignable to return type `((Any, ISortPrettyPrinter, /) -> str, /) -> (Any, ISortPrettyPrinter, /) -> str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/main.py:1126:77: Object of type `Literal[_preconvert]` cannot be assigned to parameter `default` of function `dumps`; expected type `((Any, /) -> Any) | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/isort/isort/settings.py:749:9: Object of type `Literal[from_string]` is not assignable to `((str, /) -> Any) | type[Any]`
- Found 53 diagnostics
+ Found 50 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/black/trans.py:2482:12: Object of type `Literal[insert_str_child]` is not assignable to return type `(@Todo(Inference of subscript on special form), /) -> None`
- Found 212 diagnostics
+ Found 211 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-parameter-default] /tmp/mypy_primer/projects/rich/rich/cells.py:51:25: Default value of type `Literal[cached_cell_len]` is not assignable to annotated parameter type `(str, /) -> int`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/pretty.py:226:9: Object of type `Literal[display_hook]` is not assignable to attribute `displayhook` of type `(object, /) -> Any`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:512:16: Object of type `Literal[_replace]` is not assignable to return type `(...) -> Group`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:514:12: Object of type `Literal[decorator]` is not assignable to return type `(...) -> (...) -> Group`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/traceback.py:207:9: Object of type `Literal[excepthook]` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
- Found 583 diagnostics
+ Found 578 diagnostics

```
</details>


---

_Label `red-knot` added by @dhruvmanila on 2025-03-31 15:15_

---

_@dhruvmanila reviewed on 2025-03-31 15:19_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:502 on 2025-03-31 15:19_

These two code snippets are merged by the test runner because they're under the same heading so we can simplify them by using a single code block (remove the duplicate imports, use different names for both functions).

---

_@dhruvmanila reviewed on 2025-03-31 15:34_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:479 on 2025-03-31 15:34_

I think I'd name the header as "Function types" as the "Callable" section is mostly for non-static / gradual types because the cases with fully static types are included in `is_fully_static` test cases.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:490 on 2025-03-31 15:37_

I don't think this comments adds any additional context so it might be best to remove it
```suggestion
```

---

_@dhruvmanila approved on 2025-03-31 15:37_

Looks good, thanks!

---

_Merged by @dhruvmanila on 2025-03-31 15:42_

---

_Closed by @dhruvmanila on 2025-03-31 15:42_

---

_Branch deleted on 2025-03-31 15:43_

---
