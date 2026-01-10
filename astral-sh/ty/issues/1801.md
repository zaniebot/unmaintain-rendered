```yaml
number: 1801
title: "panic: assertion `left == right` failed with invalid typing.Annotated usage"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-07T21:23:42Z
updated_at: 2025-12-08T15:24:07Z
url: https://github.com/astral-sh/ty/issues/1801
synced_at: 2026-01-10T01:56:41Z
```

# panic: assertion `left == right` failed with invalid typing.Annotated usage

---

_Issue opened by @correctmost on 2025-12-07 21:23_

### Summary

ty crashes when checking this code:

```python
from typing import Annotated

A:[Annotated, Annotated[]]
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder/type_expression.rs:1175:22 when checking `/home/user/ann.py`: `assertion `left == right` failed
  left: Some(Dynamic(Unknown))
 right: None`
```

### Version

84a18811 w/ astral-sh/ruff@857fd4f

---

_Label `fatal` added by @AlexWaygood on 2025-12-07 21:52_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-07 21:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-07 23:10_

---

_Label `bug` added by @AlexWaygood on 2025-12-08 08:20_

---

_Closed by @charliermarsh on 2025-12-08 15:24_

---
