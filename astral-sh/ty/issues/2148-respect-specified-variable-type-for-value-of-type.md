```yaml
number: 2148
title: Respect specified variable type for value of type Any or Unknown
type: issue
state: closed
author: kuyugama
labels: []
assignees: []
created_at: 2025-12-21T18:20:01Z
updated_at: 2025-12-21T18:20:58Z
url: https://github.com/astral-sh/ty/issues/2148
synced_at: 2026-01-12T15:54:26Z
```

# Respect specified variable type for value of type Any or Unknown

---

_@kuyugama_

If the value has an `Any` or `Unknown` type and variable with that value has specified type - respect the variable type

Current behavior - variable's value type stays it's original type (`Any` or `Unknown`)
Expected behavior - variable's value type changes to variable specific type

Code I've tested it with:
```python
from typing import Literal, reveal_type, Any


def app() -> Any:
    return "123"


x: Literal["123"] = app()

reveal_type(x)
```

`ty check` run output:
```
info[revealed-type]: Revealed type
  --> play.py:10:13
   |
 8 | x: Literal["123"] = app()
 9 |
10 | reveal_type(x)
   |             ^ `Any`
   |

Found 1 diagnostic
```

---

_Comment by @AlexWaygood on 2025-12-21 18:20_

Thanks! Please see #136

---

_Closed by @AlexWaygood on 2025-12-21 18:20_

---
