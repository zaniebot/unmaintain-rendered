```yaml
number: 586
title: generic dataclass instantiation fails without explicitly specifying generic type
type: issue
state: closed
author: aschulte201
labels:
  - dataclasses
assignees: []
created_at: 2025-06-05T13:19:52Z
updated_at: 2025-06-05T16:21:07Z
url: https://github.com/astral-sh/ty/issues/586
synced_at: 2026-01-12T15:54:23Z
```

# generic dataclass instantiation fails without explicitly specifying generic type

---

_@aschulte201_

### Summary

First of all, thank you for your great work with ty. I really like using your tools 😃 

### Description

I encountered an issue when instantiating a generic dataclass without explicitly specifying the type parameter. ty then reports an error like:

> Expected T, found Attr

This occurs even though the provided argument for the dataclass attribute (which was typed with the generic) is an instance of the typevars upper bound.

### Minimal example

```python
from dataclasses import dataclass

class Attr: ...

@dataclass
class Payload[T: Attr]:
    attr: T

if __name__ == "__main__":
    a = Payload(attr=Attr())
    b: Payload[Attr] = Payload(attr=Attr())
    c: Payload[Attr] = Payload[Attr](attr=Attr())
```

`a` an `b` both produce the error: `Expected 'T', found 'Attr'`.  `c` outputs no error.

### Expected behaviour

I would expect that `a` and `b` should also be valid, as `Attr` satisfies the constraint `T: Attr`, and this pattern works in mypy without issues.

### Setup Details

I was unable to reproduce the issue on the playground, but below are the details of my local setup:

* Minimal `pyproject.toml` without settings other than `project`
* Python 3.12.10
* ty 0.0.1-alpha.8 (c1337c962 2025-06-02)
* mypy 1.16.0

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `dataclasses` added by @AlexWaygood on 2025-06-05 16:21_

---

_Comment by @AlexWaygood on 2025-06-05 16:21_

Thanks for the report! This was fixed recently in https://github.com/astral-sh/ruff/commit/e8ea40012afda872ef9bdf496a8b888ba80b533b, which is why you can't reproduce it in the playground. A release should be out with it shortly!

---

_Closed by @AlexWaygood on 2025-06-05 16:21_

---
