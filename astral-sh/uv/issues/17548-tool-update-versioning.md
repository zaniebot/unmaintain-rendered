```yaml
number: 17548
title: Tool update versioning
type: issue
state: open
author: klonuo
labels:
  - bug
assignees: []
created_at: 2026-01-17T13:41:52Z
updated_at: 2026-01-17T17:27:23Z
url: https://github.com/astral-sh/uv/issues/17548
synced_at: 2026-01-17T18:19:20Z
```

# Tool update versioning

---

_@klonuo_

_To state that I'm aware of `uvx` and understand excellent workflow it provides, but for some tools I prefer reading changelog before installing_

---

I use global `ruff` and `ty` tools, and update when I see new version
If I have editor opened that registers LSP, I had issue with updating `ty` but never `ruff`

```
❯ uv tool update ruff
Updated ruff v0.14.11 -> v0.14.13
 - ruff==0.14.11
 + ruff==0.14.13
error: Failed to upgrade ruff
  Caused by: Failed to install entrypoint
  Caused by: failed to copy file from C:\Users\klo\AppData\Roaming\uv\tools\ruff\Scripts\ruff.exe to C:\Users\klo\.local\bin\ruff.exe: The process cannot access the file because it is being used by another process. (os error 32)
```

I closed processes that were using ruff, and tried again. And what happens is what I noticed with `ty` too in such scenario:

```
❯ uv tool update ruff
Nothing to upgrade
~
❯ ruff --version
ruff 0.14.11
```

What happens that `uv` can't see that there is update in this case?



---

_Comment by @zanieb on 2026-01-17 17:27_

I think this is a bug, I'll look into it.

---

_Label `bug` added by @zanieb on 2026-01-17 17:27_

---
