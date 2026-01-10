---
number: 3525
title: ERA001 false negative
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2023-03-14T21:13:48Z
updated_at: 2023-06-13T16:56:52Z
url: https://github.com/astral-sh/ruff/issues/3525
synced_at: 2026-01-10T01:22:42Z
---

# ERA001 false negative

---

_Issue opened by @JonathanPlasse on 2023-03-14 21:13_

```python
# if severity != logSeverity.DEBUG:
#    add_log(context, str(x), severity)

# with keyboard.Events() as events:
#     # Block for as much as possible
#     event = events.get(1e6)
#     if event.key == keyboard.KeyCode.from_char('q'):
#         print("goodbye")
#         raise KeyboardInterrupt
#     if event.key == keyboard.KeyCode.from_char('r'):
#         print("reloading servers...")
#         return
```
become
```python
# if severity != logSeverity.DEBUG:

# with keyboard.Events() as events:
#     # Block for as much as possible
#     if event.key == keyboard.KeyCode.from_char('q'):
#         raise KeyboardInterrupt
#     if event.key == keyboard.KeyCode.from_char('r'):
```
The upstream eradicate does not fix it either.

---

_Label `bug` added by @charliermarsh on 2023-03-14 21:51_

---

_Comment by @charliermarsh on 2023-03-16 00:08_

Lots of spooky regex in there...

---

_Comment by @charliermarsh on 2023-06-13 13:55_

I'm gonna merge with #3525.

---

_Closed by @charliermarsh on 2023-06-13 13:55_

---

_Comment by @calumy on 2023-06-13 16:49_

> I'm gonna merge with #3525.

This might be a typo as it is merging with itself.

---

_Comment by @charliermarsh on 2023-06-13 16:56_

Thanks, I meant #4845.

---
