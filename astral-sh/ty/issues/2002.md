```yaml
number: 2002
title: Ty and None values usage unclear
type: issue
state: closed
author: laurensnaturalis
labels:
  - question
assignees: []
created_at: 2025-12-17T12:14:50Z
updated_at: 2025-12-17T13:14:41Z
url: https://github.com/astral-sh/ty/issues/2002
synced_at: 2026-01-10T01:55:00Z
```

# Ty and None values usage unclear

---

_Issue opened by @laurensnaturalis on 2025-12-17 12:14_

### Question

When doing 

```py
other_probs: NDArray = None
if other_probs is not None:
    other_probs[indices] = ...
```

I get

```
Object of type `None` is not assignable to `ndarray[Any, dtype[Unknown]]`
```

for the first line. But when doing

```py
other_probs: NDArray | None  = None
if other_probs is not None:
    other_probs[indices] = ...
```

I get on the 3rd line

```
Cannot assign to a subscript on an object of type `None` ty(invalid-assignment)
```

I'm getting started with ty but I do this kind of stuff in Python all the time. I don't know how to fix this without using ignore statements.

### Version

ty 0.02

---

_Label `question` added by @laurensnaturalis on 2025-12-17 12:14_

---

_Comment by @bradezard131 on 2025-12-17 12:54_

> ```py
> other_probs: NDArray | None = None
> if other_probs is None:
>     other_probs[indices] = ...
> ```

Have you just flubbed this and meant `if other probs is not None`? Indexing into `None` will throw an exception, the error looks correct to me

---

_Comment by @laurensnaturalis on 2025-12-17 12:57_

Sorry, yes, it should be `not None`.

My problem is that I don't know how to write (the now correct code) without error messages from `ty`

Here the screenshots from pycharm with ty as language server

<img width="693" height="249" alt="Image" src="https://github.com/user-attachments/assets/6e661de2-52cd-4f2a-a222-7dab3096cc1f" />

<img alt="Image" src="https://github.com/user-attachments/assets/730f762d-c2e9-468b-b5b7-91336d026ea7" />


---

_Comment by @bradezard131 on 2025-12-17 13:04_

You have the same mistake in your code: `if other_probs is None` instead of `if other_probs is not None`. If you have the condition correct then it works fine with the `NDArray | None` type, at least from what I can see:

```
❯ cat code.py
from numpy.typing import NDArray

other_probs: NDArray | None  = None
if other_probs is not None:
    other_probs[indices] = 1

❯ uvx ty check
All checks passed!
```

---

_Closed by @laurensnaturalis on 2025-12-17 13:14_

---
