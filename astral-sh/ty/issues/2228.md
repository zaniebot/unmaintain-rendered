```yaml
number: 2228
title: "Dynamically created class type don't match against parent class"
type: issue
state: closed
author: condemil
labels: []
assignees: []
created_at: 2025-12-26T14:58:29Z
updated_at: 2025-12-26T17:03:24Z
url: https://github.com/astral-sh/ty/issues/2228
synced_at: 2026-01-10T01:56:41Z
```

# Dynamically created class type don't match against parent class

---

_Issue opened by @condemil on 2025-12-26 14:58_

### Summary

When class is created dynamically with `type(name, bases, dict)` it's type evaluates as just `type`. That prevents it to match against base class type:

```python
from unittest import TestCase

tests: list[type[TestCase]] = []
testCaseClass = type("Foo", (TestCase,), {})

# Argument to bound method `append` is incorrect: Expected `type[TestCase]`, found `type`
tests.append(testCaseClass)
```

### Version

_No response_

---

_Comment by @carljm on 2025-12-26 17:03_

Thanks! This is #740

---

_Closed by @carljm on 2025-12-26 17:03_

---
