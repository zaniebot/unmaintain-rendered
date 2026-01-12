```yaml
number: 4082
title: E714 Complex logic handling
type: issue
state: closed
author: justinchuby
labels:
  - bug
assignees: []
created_at: 2023-04-24T16:20:04Z
updated_at: 2023-04-25T17:37:57Z
url: https://github.com/astral-sh/ruff/issues/4082
synced_at: 2026-01-12T15:54:44Z
```

# E714 Complex logic handling

---

_@justinchuby_

Example

```python
if not (index.start is index.stop is index.step is None):
    pass
```

raises E714

<img width="604" alt="image" src="https://user-images.githubusercontent.com/11205048/234056372-378889d2-6112-4eb7-a9f9-0c5418b1d1ef.png">

but it is not straightforward to see what the fix should be or if it is a better representation of the logic.


---

_Renamed from "Complex logic handling with E714" to "E714 Complex logic handling" by @justinchuby on 2023-04-24 16:20_

---

_Comment by @JonathanPlasse on 2023-04-24 19:09_

I think it should be ignored like for the following case.
```python
if not (index.start == index.stop == index.step == None):
    pass
```
I can pick it up.

---

_Label `bug` added by @charliermarsh on 2023-04-25 00:13_

---

_Closed by @charliermarsh on 2023-04-25 17:37_

---
