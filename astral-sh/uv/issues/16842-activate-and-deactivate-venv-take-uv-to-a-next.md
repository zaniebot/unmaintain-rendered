```yaml
number: 16842
title: Activate and deactivate venv (take uv to a next level of convenience)
type: issue
state: closed
author: bernd-wechner
labels:
  - enhancement
assignees: []
created_at: 2025-11-25T07:15:08Z
updated_at: 2025-11-26T16:40:57Z
url: https://github.com/astral-sh/uv/issues/16842
synced_at: 2026-01-12T16:02:39Z
```

# Activate and deactivate venv (take uv to a next level of convenience)

---

_@bernd-wechner_

### Summary

I think it would be awesome if uv remembered the last venv it created (as an environment variable, or in a dot file, it matters not, because the aim simply that afterward `uv venv activate` would activate it. A universal simple shortcut. And then `uv venv deactivate` would deactivate it.  That woudl of course predicate activate and deactviate as keywords not venv names! I find that a livable compromise but am happy to hear other ideas. 

In fact it goes one better, if it remembers the last venv it deactivated again, reactivation is a doddle. 

To my mind this would remove my clunkiness form python venv management. 

### Example

`uv venv activate`
`uv venv deactivate`

---

_Label `enhancement` added by @bernd-wechner on 2025-11-25 07:15_

---

_Comment by @harshil21 on 2025-11-25 08:21_

have you looked at `uv run`? It manages the environment completely for you.

---

_Comment by @konstin on 2025-11-26 15:59_

See https://github.com/astral-sh/uv/issues/1910 for something very similar, and the technical reasons why uv currently doesn't activate the venv for you.

---

_Closed by @zanieb on 2025-11-26 16:40_

---
