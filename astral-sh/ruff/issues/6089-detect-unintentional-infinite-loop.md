---
number: 6089
title: Detect unintentional infinite loop
type: issue
state: closed
author: berzi
labels: []
assignees: []
created_at: 2023-07-26T09:17:35Z
updated_at: 2023-08-04T02:57:59Z
url: https://github.com/astral-sh/ruff/issues/6089
synced_at: 2026-01-10T01:22:45Z
---

# Detect unintentional infinite loop

---

_Issue opened by @berzi on 2023-07-26 09:17_

Consider the following code:
```python
counter: int = 0
while counter < 100:
    do_stuff()
```

You may notice that `counter` is not actually changed at all within the loop, and since `int`s are immutable, whatever happens inside `do_stuff()` cannot change it either. This means it is statically possible to determine that this loop will not stop, and I think it would make for a very useful lint rule, since in more complex scenarios it's very easy for a human (or indeed a dumb AI) to forget to change the loop condition variable.

This should be perfectly achievable for any immutable type, and perhaps also for mutable ones to some extent (if the mutable variable isn't used inside the loop at all, we know that it won't be changed; this leaves the possibility open that the variable is passed down some function which does not end up changing it, but at least we've covered what human error we could).

---

_Comment by @charliermarsh on 2023-08-04 02:57_

I think this would require enough new control-flow analysis that we're unlikely to tackle it soon, and I want to keep the issue tracker actionable, so I'm gonna bias towards saying "not now", for now. We may revisit in the future once we've improved our control-flow analysis capabilities.

---

_Closed by @charliermarsh on 2023-08-04 02:57_

---
