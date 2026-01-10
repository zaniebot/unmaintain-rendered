```yaml
number: 16907
title: "[red-knot] Fix disambiguate display for union types"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-callable-union-display
created_at: 2025-03-22T00:34:51Z
updated_at: 2025-03-22T12:21:20Z
url: https://github.com/astral-sh/ruff/pull/16907
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Fix disambiguate display for union types

---

_Pull request opened by @MatthewMckee4 on 2025-03-22 00:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When callables are displayed in unions, like:
```py
from typing import Callable


def foo(x: Callable[[], int] | None):
    # red-knot: Revealed type is `() -> int | None` [revealed-type]
    reveal_type(x)
```

This leaves the type rather ambiguous, to fix this we can add parenthesis to callable type in union

Fixes #16893

## Test Plan

Update callable annotations tests


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-22 00:34_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-22 00:34_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-22 00:34_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-22 00:34_

---

_Comment by @github-actions[bot] on 2025-03-22 00:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
isort (https://github.com/pycqa/isort)
- error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/sorting.py:120:34: Object of type `(str, /) -> Any | None` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/isort/isort/sorting.py:120:34: Object of type `((str, /) -> Any) | None` is not callable
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/isort/isort/settings.py:749:9: Object of type `Literal[from_string]` is not assignable to `((str, /) -> Any) | type[Any]`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/isort/isort/settings.py:749:9: Object of type `Literal[from_string]` is not assignable to `(str, /) -> Any | type[Any]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:55:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:55:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `((str, /) -> Any) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:66:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:66:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `((str, /) -> Any) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:113:17: Object of type `partial` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:113:17: Object of type `partial` cannot be assigned to parameter `key` of function `sort`; expected type `((str, /) -> Any) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:268:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `(str, /) -> Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/output.py:268:17: Object of type `(key) -> Unknown` cannot be assigned to parameter `key` of function `sort`; expected type `((str, /) -> Any) | None`

rich (https://github.com/Textualize/rich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/tests/test_progress.py:292:60: Object of type `MockClock` cannot be assigned to parameter `get_time` of function `track`; expected type `() -> int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/tests/test_progress.py:292:60: Object of type `MockClock` cannot be assigned to parameter `get_time` of function `track`; expected type `(() -> int | float) | None`

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_threaded.py:37:24: Object of type `CallCounter` cannot be assigned to parameter 1 (`target`) of function `setstatprofile`; expected type `((FrameType, str, Any, /) -> Any) | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:28:5: Object of type `@Todo | <bound method `get_time` of `FakeClock`>` is not assignable to attribute `timer_func` of type `() -> int | float | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:28:5: Object of type `@Todo | <bound method `get_time` of `FakeClock`>` is not assignable to attribute `timer_func` of type `(() -> int | float) | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_threaded.py:37:24: Object of type `CallCounter` cannot be assigned to parameter 1 (`target`) of function `setstatprofile`; expected type `(FrameType, str, Any, /) -> Any | None`

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-03-22 01:01_

---

_@InSyncWithFoo reviewed on 2025-03-22 02:49_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:169 on 2025-03-22 02:49_

What about intersection types?

```python
def _(
    c: Intersection[Callable[[Union[int, str]], int], A],
    d: Intersection[A, Callable[[Union[int, str]], int]],
    e: Intersection[A, Callable[[Union[int, str]], int], B],
    f: Intersection[Not[Callable[[int, str], Intersection[int, str]]]]
):
    reveal_type(c)  # revealed: ((int | str, /) -> int) & A
    reveal_type(d)  # revealed: A & ((int | str, /) -> int)
    reveal_type(e)  # revealed: A & ((int | str, /) -> int) & B
    reveal_type(f)  # revealed: ~((int, str, /) -> int & str)
```

---

_@MichaReiser reviewed on 2025-03-22 09:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:299 on 2025-03-22 09:55_

Ah, I wish [`from_fn`](https://doc.rust-lang.org/std/fmt/fn.from_fn.html) gets stabilizied. It would allow to remove the `format!` without much ceremony.... 

Although, I think you can avoid the `format` call here by using `format_args` (but not a 100% sure)
```suggestion
                    join.entry(&format_args!("({})", element.display(self.db)));
```

---

_@MichaReiser approved on 2025-03-22 09:55_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:169 on 2025-03-22 11:24_

Good point, right now revealed type of f is `~(int, str, /) -> int & str`". We can probably do this in another PR?

---

_@MatthewMckee4 reviewed on 2025-03-22 11:24_

---

_@MichaReiser reviewed on 2025-03-22 12:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/callable.md`:169 on 2025-03-22 12:07_

Yeah, I think it's fine to do this in another PR.

---

_Merged by @MichaReiser on 2025-03-22 12:08_

---

_Closed by @MichaReiser on 2025-03-22 12:08_

---

_Branch deleted on 2025-03-22 12:21_

---
