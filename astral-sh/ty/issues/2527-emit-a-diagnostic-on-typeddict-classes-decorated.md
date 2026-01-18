```yaml
number: 2527
title: "Emit a diagnostic on `TypedDict` classes decorated with `@dataclass`"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - dataclasses
  - typeddict
assignees: []
created_at: 2026-01-16T02:02:56Z
updated_at: 2026-01-18T17:20:11Z
url: https://github.com/astral-sh/ty/issues/2527
synced_at: 2026-01-18T18:15:23Z
```

# Emit a diagnostic on `TypedDict` classes decorated with `@dataclass`

---

_@MeGaGiGaGon_

This should be an error, since using the resulting class like a `dataclass` will always fail at runtime. It would also be a useful diagnostic to have because it can happen (and has to me a couple times) while refactoring a `dataclass` to a `TypedDict`, but forgetting to remove the decorator. Maybe could be extended to all decorators? Unsure, but `dataclass` would be the most useful.
https://play.ty.dev/2ddb5461-df08-43fc-88d6-8be63319220b
```py
from typing import TypedDict
from dataclasses import dataclass

@dataclass
class Foo(TypedDict):
    x: int

print(Foo(1))  # No error because ty thinks it is a dataclass, but fails at runtime
```

---

_Label `dataclasses` added by @AlexWaygood on 2026-01-16 08:26_

---

_Label `typeddict` added by @AlexWaygood on 2026-01-16 08:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-17 20:07_

---

_Closed by @charliermarsh on 2026-01-18 17:20_

---
