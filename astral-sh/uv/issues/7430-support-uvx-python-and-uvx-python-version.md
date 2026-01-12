```yaml
number: 7430
title: "Support `uvx python` and `uvx python@<version>`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - help wanted
  - uv tool
assignees: []
created_at: 2024-09-16T14:50:31Z
updated_at: 2025-01-30T17:53:59Z
url: https://github.com/astral-sh/uv/issues/7430
synced_at: 2026-01-12T15:59:13Z
```

# Support `uvx python` and `uvx python@<version>`

---

_@zanieb_

To improve the experience of spawning an interactive REPL

Related

- https://github.com/astral-sh/uv/issues/6548

---

_Label `uv tool` added by @zanieb on 2024-09-16 14:50_

---

_Label `enhancement` added by @zanieb on 2024-09-16 14:50_

---

_Label `help wanted` added by @zanieb on 2024-09-16 15:59_

---

_Comment by @Aditya-PS-05 on 2024-09-17 13:52_

@zanieb,
According to question -> support ```uvx python```
Internally it will be work as ```uv run python```. 

Am I corrected? Is there something else?

---

_Comment by @zanieb on 2024-09-17 14:37_

`uvx python` is an alias for `uv tool run python` which would create an isolated empty environment. `uv run python` will use existing environments if it can find it — it's a bit different in that sense.

---

_Comment by @mikeleppane on 2024-09-17 15:43_

@zanieb If it's okay, I would be interested in taking a look at this 🕵‍♂️. If possible, could you give some hints on where to start with this?

---

_Comment by @zanieb on 2024-09-17 15:48_

@mikeleppane sweet! Probably around https://github.com/astral-sh/uv/blob/4f2349119cf341eedf738d06a50ed136a5f207db/crates/uv/src/commands/tool/run.rs#L96

I'm not sure what the best way to refactor it is. Maybe we want a new `Target` variant `Python` which we then special-case?

---

_Comment by @zanieb on 2024-09-17 15:50_

Then we'd have to introduce more variants for `python@latest` and such though, so maybe we just want to special case the "python" name around https://github.com/astral-sh/uv/blob/4f2349119cf341eedf738d06a50ed136a5f207db/crates/uv/src/commands/tool/run.rs#L352-L404

---

_Closed by @zanieb on 2025-01-30 17:53_

---
