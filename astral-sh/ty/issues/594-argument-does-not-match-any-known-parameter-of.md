```yaml
number: 594
title: "Argument does not match any known parameter of bound method `__init__`"
type: issue
state: closed
author: Ryang20718
labels:
  - needs-info
assignees: []
created_at: 2025-06-06T18:14:43Z
updated_at: 2025-06-07T14:14:17Z
url: https://github.com/astral-sh/ty/issues/594
synced_at: 2026-01-12T15:54:23Z
```

# Argument does not match any known parameter of bound method `__init__`

---

_@Ryang20718_

### Summary

Reproable via the logset_uri typing errors with

`error[unknown-argument]: Argument `logset_uri` does not match any known parameter of bound method `__init__``

```
import serde
from pydantic.dataclasses import dataclass as dataclass_with_validation
from typing import Optional

@serde.serde
@dataclass_with_validation
class LogSet:
    logset_uri: Optional[str] = None

x = LogSet(logset_uri="https://example.com/logset")

```

### Version

0.0.1a8

---

_Comment by @carljm on 2025-06-07 01:52_

Hi! Thanks for the report. Can you clarify what library the `serde.serde` decorator used here comes from?

With that decorator removed, the issue doesn't reproduce.

There is a library named "serde" on PyPI, but it doesn't appear to have such a decorator.

If the `serde` module is part of your own code, we'll need to see the definition of that decorator in order to understand the issue here.

If I define a simple pass-through no-op decorator `def serde[T](cls: T) -> T: return cls` and apply that, the issue also doesn't reproduce. So it seems like the problem is related to the definition of your `serde` decorator.

---

_Label `needs-info` added by @carljm on 2025-06-07 01:52_

---

_Comment by @Ryang20718 on 2025-06-07 06:48_

we're using `pyserde[toml,yaml]==0.9.2`

---

_Comment by @Ryang20718 on 2025-06-07 14:14_

https://github.com/astral-sh/ty/issues/598#issuecomment-2951942220

---

_Closed by @Ryang20718 on 2025-06-07 14:14_

---
