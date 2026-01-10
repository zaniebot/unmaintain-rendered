```yaml
number: 14110
title: SIM115 triggers when an ExitStack is passed into a function
type: issue
state: open
author: tjstum
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-11-05T15:32:31Z
updated_at: 2024-11-05T16:35:49Z
url: https://github.com/astral-sh/ruff/issues/14110
synced_at: 2026-01-10T11:09:55Z
```

# SIM115 triggers when an ExitStack is passed into a function

---

_Issue opened by @tjstum on 2024-11-05 15:32_

I am using ruff 0.7.2. I searched for  SIM115 and `ExitStack` before filing this issue (and found #1945).

If you have code like this:
```python
from contextlib import ExitStack

with ExitStack() as stack:
    stack.enter_context(open("/path/to/file"))
```
SIM115 does not trigger. However, if the stack object is passed into the function (correctly annotated), the message is issued:
```python
from contextlib import ExitStack

def open_the_file_elsewhere(exit_stack: ExitStack) -> None:
    exit_stack.enter_context(open("/path/to/file"))
```

I'm not sure to what extent ruff follows annotated function arguments, but I would expect this pattern not to trigger the message.

---

_Label `type-inference` added by @AlexWaygood on 2024-11-05 16:34_

---

_Label `bug` added by @AlexWaygood on 2024-11-05 16:35_

---

_Comment by @AlexWaygood on 2024-11-05 16:35_

Thanks, I agree that this is definitely something we _should_ support. However, it won't be possible until we have more advanced type inference. This is something we're actively working on, but it will be a while before it arrives, I'm afraid.

---
