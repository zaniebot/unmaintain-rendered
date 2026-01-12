```yaml
number: 16888
title: "[red-knot] Support calling a `typing.Callable`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-call
created_at: 2025-03-21T09:09:56Z
updated_at: 2025-05-07T15:21:03Z
url: https://github.com/astral-sh/ruff/pull/16888
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Support calling a `typing.Callable`

---

_@dhruvmanila_

## Summary

Part of astral-sh/ruff#15382, this PR adds support for calling a variable that's annotated with `typing.Callable`.

## Test Plan

Add test cases in a new `call/annotation.md` file.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-21 09:09_

---

_Comment by @github-actions[bot] on 2025-03-21 09:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:32:16: Object of type `() -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/metrics/overhead.py:45:24: Object of type `() -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/middleware.py:50:13: Object of type `(request) -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/test/util.py:116:13: Object of type `(...) -> Unknown` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:41:50: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:42:57: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:43:51: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:58:54: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:61:56: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:72:60: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/frame.py:349:52: Object of type `(str, /) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/util.py:36:12: Object of type `(...) -> Any` is not callable
- Found 300 diagnostics
+ Found 288 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/sorting.py:120:34: Object of type `((str, /) -> Any) | None` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/wrap.py:30:17: Object of type `(...) -> str` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/sorting.py:120:34: Object of type `None` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/wrap.py:53:36: Object of type `(...) -> str` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/output.py:170:18: Object of type `(str, str, object, /) -> str` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/output.py:170:18: Object of type `None` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/literal.py:65:29: Object of type `None` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/literal.py:65:29: Object of type `(str, str, object, /) -> str` is not callable
- Found 78 diagnostics
+ Found 76 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/encoding.py:54:12: Object of type `(bytes, /) -> tuple[int]` is not callable
- Found 56 diagnostics
+ Found 55 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_console.py:685:12: Object of type `() -> int | float` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_console.py:686:12: Object of type `() -> datetime` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_cells.py:70:20: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_cells.py:75:16: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_cells.py:78:20: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/tests/test_cells.py:81:20: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/spinner.py:53:27: Object of type `() -> int | float` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/console.py:509:27: Object of type `(...) -> @Todo` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/console.py:1900:17: Object of type `() -> FrameType | None` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/cells.py:46:8: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/cells.py:61:16: Object of type `(str, /) -> int` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/cells.py:62:8: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/cells.py:99:8: Object of type `(str, /) -> bool` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/_log_render.py:57:36: Object of type `() -> datetime` is not callable
- error[lint:call-non-callable] /tmp/mypy_primer/projects/rich/rich/segment.py:173:12: Object of type `(str, /) -> bool` is not callable
- Found 604 diagnostics
+ Found 589 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/pybind11/tests/test_virtual_functions.py:280:13: Object of type `() -> Unknown` is not callable
- Found 278 diagnostics
+ Found 277 diagnostics

```
</details>


---

_Comment by @dhruvmanila on 2025-03-21 11:36_

The ecosystem checks looks good except for two things:
1. https://github.com/astral-sh/ty/issues/164
2. I think I'll need to implement `is_disjoint_from` support for the following code to work:
```py
def test(key: Optional[Callable[[str], Any]] = None) -> None:
    if key is not None:
        reveal_type(key)  # revealed: ((str, /) -> Any) | ~None

        def key_callback(text: str) -> Any:
            reveal_type(key)  # revealed: ((str, /) -> Any) | None
            # red-knot: Object of type `None` is not callable [lint:call-non-callable]
            return key(text)
```

---

_Marked ready for review by @dhruvmanila on 2025-03-21 11:37_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-21 11:37_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-21 11:37_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-21 11:37_

---

_Review requested from @dcreager by @dhruvmanila on 2025-03-21 11:37_

---

_@AlexWaygood approved on 2025-03-21 12:37_

---

_@AlexWaygood reviewed on 2025-03-21 12:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/annotation.md`:24 on 2025-03-21 12:39_

there's room for the error message to be improved here, but it's out of scope for this PR:

```suggestion
def _(c: Callable[[int, str], None]):
    # error: [unknown-argument] "Keyword argument `a` does not match any known parameter (`c` accepts no keyword arguments)"
    # error: [unknown-argument] "Keyword argument `b` does not match any known parameter (`c` accepts no keyword arguments)"
    # error: [missing-argument] "No arguments provided for required parameters 1, 2"
    reveal_type(c(a=1, b="b"))  # revealed: None
```

---

_@carljm approved on 2025-03-21 16:04_

Awesome! Tests were most of the work here, it looks like :)

---

_@dhruvmanila reviewed on 2025-03-22 21:03_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/call/annotation.md`:24 on 2025-03-22 21:03_

Yeah, totally.

---

_@dhruvmanila reviewed on 2025-03-22 21:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/call/annotation.md`:24 on 2025-03-22 21:09_

#16919, feel free to update the description or comment to provide more context and examples.

---

_Merged by @dhruvmanila on 2025-03-22 21:09_

---

_Closed by @dhruvmanila on 2025-03-22 21:09_

---

_Branch deleted on 2025-03-22 21:09_

---
