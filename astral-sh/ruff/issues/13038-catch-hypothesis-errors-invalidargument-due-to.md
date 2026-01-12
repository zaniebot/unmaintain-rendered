```yaml
number: 13038
title: "Catch `hypothesis.errors.InvalidArgument` due to multiple `@given` decorators"
type: issue
state: open
author: petermattia
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-08-21T20:41:23Z
updated_at: 2024-08-23T05:33:36Z
url: https://github.com/astral-sh/ruff/issues/13038
synced_at: 2026-01-12T15:54:52Z
```

# Catch `hypothesis.errors.InvalidArgument` due to multiple `@given` decorators

---

_@petermattia_

List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
- "hypothesis"

It would be cool if `ruff` could automatically detect this `hypothesis` issue. Much faster to detect statically than during testing.

````python
Catch FAILED src/tests/test_logic.py::test_check_value_is_greater_than_zero - hypothesis.errors.InvalidArgument: You have applied @given to the test test_check_value_is_greater_than_zero more than once, which wraps the test several times and is extremely slow. A similar effect can be gained by combining the arguments of the two calls to given. For example, instead of @given(booleans()) @given(integers()), you could write @given(booleans(), integers())
````

---

_Renamed from "Catch FAILED scan_processing/scan_info/tests/test_slicing_logic.py::test_check_value_is_greater_than_zero - hypothesis.errors.InvalidArgument: You have applied @given to the test test_check_value_is_greater_than_zero more than once, which wraps the test several times and is extremely slow. A similar effect can be gained by combining the arguments of the two calls to given. For example, instead of @given(booleans()) @given(integers()), you could write @given(booleans(), integers())" to "Catch `hypothesis.errors.InvalidArgument`" by @petermattia on 2024-08-21 20:41_

---

_Renamed from "Catch `hypothesis.errors.InvalidArgument`" to "Catch `hypothesis.errors.InvalidArgument` due to multiple `@given` decorators" by @petermattia on 2024-08-21 20:41_

---

_Comment by @dhruvmanila on 2024-08-23 05:30_

I'm not familiar with hypothesis but I wonder if a codemod is available for this using https://hypothesis.readthedocs.io/en/latest/extras.html#hypothesis-codemods.

---

_Label `rule` added by @dhruvmanila on 2024-08-23 05:30_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-23 05:30_

---

_Comment by @dhruvmanila on 2024-08-23 05:33_

Note: https://github.com/astral-sh/ruff/issues/7272#issuecomment-1714819109

---
