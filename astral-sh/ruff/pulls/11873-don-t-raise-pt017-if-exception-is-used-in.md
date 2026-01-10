```yaml
number: 11873
title: "Don't raise PT017 if exception is used in assertion message"
type: pull_request
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
base: main
head: fix-pt017
created_at: 2024-06-14T13:23:23Z
updated_at: 2024-08-12T07:54:22Z
url: https://github.com/astral-sh/ruff/pull/11873
synced_at: 2026-01-10T21:38:31Z
```

# Don't raise PT017 if exception is used in assertion message

---

_Pull request opened by @MichaReiser on 2024-06-14 13:23_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11869


## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2024-06-14 13:23_

---

_Comment by @github-actions[bot] on 2024-06-14 13:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-06-14 13:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT017.py`:21 on 2024-06-14 13:53_

This isn't a great example, because it's not valid Python. The `e` binding doesn't last outside the `except` block:

```pycon
>>> def allow_exception_in_message():
...     try:
...         something()
...     except Exception as e:
...         something_else()
...     print(e)
... 
>>> def something(): raise Exception
... 
>>> def something_else(): pass
... 
>>> allow_exception_in_message()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 6, in allow_exception_in_message
UnboundLocalError: cannot access local variable 'e' where it is not associated with a value
```

---

_@MichaReiser reviewed on 2024-06-14 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT017.py`:21 on 2024-06-14 14:01_

I updated the example

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT017.py`:20 on 2024-06-14 14:04_

To me this feels like exactly the kind of thing the rule is trying to catch, rather than something we should avoid emitting a diagnostic for: I commented at https://github.com/astral-sh/ruff/issues/11869#issuecomment-2168117376. If you want to use the value of the exception in the assertion message, you can do that using `pytest.raises()`:

```py
def allow_exception_in_message():
    x = 0
    with pytest.raises(ZeroDivisionError) as exc_info:
        1 / x
    assert x == 0, exc_info.value
```

---

_@AlexWaygood reviewed on 2024-06-14 14:04_

---

_@zanieb reviewed on 2024-06-14 14:43_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT017.py`:20 on 2024-06-14 14:43_

I'm leaning towards agreeing with Alex here

---

_Closed by @MichaReiser on 2024-06-14 15:41_

---

_Branch deleted on 2024-08-12 07:54_

---
