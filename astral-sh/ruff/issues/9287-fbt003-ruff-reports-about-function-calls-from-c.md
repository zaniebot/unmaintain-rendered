```yaml
number: 9287
title: "FBT003: ruff reports about function calls from C-extentions"
type: issue
state: closed
author: Paciupa
labels: []
assignees: []
created_at: 2023-12-26T20:27:08Z
updated_at: 2024-01-08T18:02:18Z
url: https://github.com/astral-sh/ruff/issues/9287
synced_at: 2026-01-12T15:54:49Z
```

# FBT003: ruff reports about function calls from C-extentions

---

_@Paciupa_

In my case this is calls from `pygame`, for example:
```python
pygame.mouse.set_visible(False)
```
Ruff reports `FBT003 Boolean positional value in function call`.
If I try use keyword argument, I'll get ```TypeError: set_visible() takes no keyword arguments```
And it happens with every function from every C-library that takes bool.
So the only solution now is suppress this message.
Ruff version: 0.1.9.

---

_Comment by @charliermarsh on 2023-12-26 21:57_

I believe this is a duplicate of https://github.com/astral-sh/ruff/issues/3247. There aren't many good solutions here -- I typically recommend just not using the `FBT` rules (which are quite rigid and opinionated) if you're not working in a fairly strict project or setting.

---

_Closed by @charliermarsh on 2023-12-26 21:57_

---

_Comment by @Paciupa on 2023-12-26 22:45_

Ok, thanks

---

_Closed by @charliermarsh on 2024-01-08 18:02_

---
