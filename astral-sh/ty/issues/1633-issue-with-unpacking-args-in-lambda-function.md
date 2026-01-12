```yaml
number: 1633
title: Issue with unpacking args in lambda function
type: issue
state: open
author: jrdnh
labels: []
assignees: []
created_at: 2025-11-25T20:15:44Z
updated_at: 2025-12-19T12:06:21Z
url: https://github.com/astral-sh/ty/issues/1633
synced_at: 2026-01-12T15:54:25Z
```

# Issue with unpacking args in lambda function

---

_@jrdnh_

### Summary

The following code raises an unexpected type error in the first snippet and does not raise a type error in the second snippet (which is invalid code).

Ideally would have hoped the required type for the arg could be inferred (#181) based on the context.

```python
# pow(x, y, modulo)
# type error on "pow", valid code
foo = lambda pair=(2, 2): pow(*pair, 4)
foo()

# no type error, causes runtime error
foo(('a', 'b'))
```

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Comment by @carljm on 2025-11-25 20:34_

Thanks for the report!

Our behavior here matches existing type checkers (pyright, mypy, and pyrefly), but I agree we would ideally prefer to do better.

This is (as you observe) closely related to #181, but it's a more "advanced" case that can't be handled with some (type-context-based) approaches to #181, so I'll keep it open as a sub-issue rather than closing as duplicate.

---

_Label `lambda` added by @carljm on 2025-11-25 20:35_

---

_Label `lambda` removed by @AlexWaygood on 2025-12-19 12:06_

---
