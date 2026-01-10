```yaml
number: 1768
title: "panic: assertion `left == right` failed when ClassVar has a tuple of types"
type: issue
state: closed
author: correctmost
labels:
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-05T12:29:18Z
updated_at: 2025-12-07T14:27:16Z
url: https://github.com/astral-sh/ty/issues/1768
synced_at: 2026-01-10T01:56:41Z
```

# panic: assertion `left == right` failed when ClassVar has a tuple of types

---

_Issue opened by @correctmost on 2025-12-05 12:29_

### Summary

ty crashes when checking this code:

```python
from typing import ClassVar

class C:
    a: ClassVar[int,] = 1
```
```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder/annotation_expression.rs:314:34 when checking `/home/user/c.py`: `assertion `left == right` failed
  left: Some(Dynamic(Unknown))
 right: None`
```

### Version

51c73d6 w/ astral-sh/ruff@3deb7e1

---

_Comment by @AlexWaygood on 2025-12-05 12:30_

Thanks! What fuzzer are you using to find these? Is it public?

---

_Label `fatal` added by @AlexWaygood on 2025-12-05 12:30_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 13:07_

---

_Comment by @correctmost on 2025-12-05 13:48_

> What fuzzer are you using to find these? Is it public?

I found this crash by mutating some existing Python samples.  Unfortunately, my fuzzing set-up is completely local for now.

---

_Comment by @AlexWaygood on 2025-12-05 14:16_

We have a fuzzer that we run in CI (we currently just use it as a baseline of things that we know we didn't previously crash on -- we don't yet run ty on randomly generated samples in CI).

Our existing fuzzer is very useful. But its strength is that it's very good at generating unusual combinations of Python _syntax_ -- it's not so good at generating unusual uses of typing-module special forms. And I think mutation-based fuzzing would probably be excellent at that kind of thing. So if you ever feel like contributing your script, I think we'd find it very useful!

---

_Added to milestone `Beta` by @carljm on 2025-12-05 15:49_

---

_Assigned to @carljm by @carljm on 2025-12-05 15:49_

---

_Unassigned @carljm by @AlexWaygood on 2025-12-07 13:36_

---

_Assigned to @charliermarsh by @AlexWaygood on 2025-12-07 13:36_

---

_Closed by @charliermarsh on 2025-12-07 14:27_

---
