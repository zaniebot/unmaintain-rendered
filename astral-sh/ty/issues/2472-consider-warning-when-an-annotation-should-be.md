```yaml
number: 2472
title: Consider warning when an annotation should be narrower
type: issue
state: open
author: jack-mcivor
labels: []
assignees: []
created_at: 2026-01-12T19:11:33Z
updated_at: 2026-01-12T19:11:33Z
url: https://github.com/astral-sh/ty/issues/2472
synced_at: 2026-01-12T19:26:07Z
```

# Consider warning when an annotation should be narrower

---

_@jack-mcivor_

I think this could catch real issues in code - for example, this currently does not error (ty v0.0.11), but is a runtime error:
```python
from typing import Sequence

def foo() -> Sequence[int]:  # <- the annotation here can be narrower - should be `tuple[Literal[5], Literal[6]]`
    return (5, 6)
foo()[2]  # bad - no ty error!
```

This variable annotation should also be made narrower. The difference here is that ty seems to ignore the annotation, so there is an error
```python
a: Sequence[int] = (5, 6)  # <- the annotation here can be narrower (and afaict unused by ty anyway)
a[2]  # good - "error[index-out-of-bounds]: Index 2 is out of bounds for tuple `tuple[Literal[5], Literal[6]]` with length 2"
```

---
