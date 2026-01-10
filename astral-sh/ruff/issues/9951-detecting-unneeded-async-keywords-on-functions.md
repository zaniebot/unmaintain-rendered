```yaml
number: 9951
title: "Detecting unneeded `async` keywords on functions"
type: issue
state: closed
author: Greesb
labels:
  - rule
assignees: []
created_at: 2024-02-12T15:06:17Z
updated_at: 2024-04-16T17:32:30Z
url: https://github.com/astral-sh/ruff/issues/9951
synced_at: 2026-01-10T11:09:52Z
```

# Detecting unneeded `async` keywords on functions

---

_Issue opened by @Greesb on 2024-02-12 15:06_

Hello,

I refactored a bunch of code and thought about a rule that could be useful: detecting function defined with the `async` keywords not using any `await` inside it, example:
```python

async def unneeded_async() -> None:  # should raise the error
    print("lol")

async def legit_async() -> None:  # should not raise the error
    await some_stuff()
```

There's probably some use cases where the async is needed, but, imo, most of the time those async can be removed.

What do you think ?

---

_Renamed from "Detected unneeded `async` keywords`" to "Detected unneeded `async` keywords on functions" by @Greesb on 2024-02-12 15:06_

---

_Comment by @zanieb on 2024-02-12 15:55_

We like this idea :) this is a duplicate of #9833 though

---

_Closed by @zanieb on 2024-02-12 15:55_

---

_Comment by @Greesb on 2024-02-12 16:46_

Doesn't look like it is, the issue you mentioned is for detecting missing `await` keywords when calling `async` functions. 

What i'm talking about is to detect functions that are defined with the `async` keyword but do not need it because there's no `await` inside them. @zanieb 

---

_Comment by @zanieb on 2024-02-12 19:23_

Yeah I see, it's a little mixed up over there as we're discussing both things.

We can track this separately.

@plredmond I'll move you over to this issue.


---

_Reopened by @zanieb on 2024-02-12 19:23_

---

_Label `rule` added by @zanieb on 2024-02-12 19:23_

---

_Renamed from "Detected unneeded `async` keywords on functions" to "Detecting unneeded `async` keywords on functions" by @Greesb on 2024-02-12 20:45_

---

_Comment by @plredmond on 2024-02-12 22:43_

In progress implementation: https://github.com/astral-sh/ruff/compare/main...plredmond:ruff:issue9951-spurious_async

---

_Closed by @plredmond on 2024-04-16 17:32_

---
