```yaml
number: 3540
title: "Custom name of directory instead of \".venv\" for multiple envs inside the same project"
type: issue
state: closed
author: detrin
labels:
  - question
assignees: []
created_at: 2024-05-13T10:29:26Z
updated_at: 2024-05-13T16:15:00Z
url: https://github.com/astral-sh/uv/issues/3540
synced_at: 2026-01-10T05:31:37Z
```

# Custom name of directory instead of ".venv" for multiple envs inside the same project

---

_Issue opened by @detrin on 2024-05-13 10:29_

I would like to create multiple environments with a custom name instead of the default ".venv" directory name. I can't find it in the README.md. Is this option available? 

---

_Comment by @zanieb on 2024-05-13 13:03_

`uv venv [name]` should work.

---

_Label `question` added by @zanieb on 2024-05-13 13:03_

---

_Comment by @detrin on 2024-05-13 13:06_

> `uv venv [name]` should work.

It does work! Thanks! Would you add it to README.md as well? I didn't find any docs outside README.md

---

_Comment by @zanieb on 2024-05-13 13:23_

We could, though I'm not sure where it'd fit right now.

The CLI help menu is pretty useful e.g. `uv venv --help`.

---

_Comment by @detrin on 2024-05-13 13:24_

I think it would fit into the section "To create a virtual environment:"

---

_Comment by @detrin on 2024-05-13 13:25_

I see we thought of the same section :)

---

_Comment by @zanieb on 2024-05-13 13:37_

Thanks!

---

_Closed by @zanieb on 2024-05-13 16:15_

---
