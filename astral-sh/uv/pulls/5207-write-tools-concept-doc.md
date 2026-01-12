```yaml
number: 5207
title: Write tools concept doc
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/tools-concept
created_at: 2024-07-19T01:35:49Z
updated_at: 2024-07-23T18:43:33Z
url: https://github.com/astral-sh/uv/pull/5207
synced_at: 2026-01-12T16:06:41Z
```

# Write tools concept doc

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-07-19 01:35_

---

_Label `preview` added by @zanieb on 2024-07-19 01:35_

---

_Marked ready for review by @zanieb on 2024-07-19 01:38_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-19 14:29_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-19 14:29_

---

_Merged by @zanieb on 2024-07-19 19:24_

---

_Closed by @zanieb on 2024-07-19 19:24_

---

_Branch deleted on 2024-07-19 19:24_

---

_Review comment by @BurntSushi on `docs/tools.md`:3 on 2024-07-23 18:39_

Some questions that come up for me after reading this:

* Does the temporary venv get deleted after `uvx` is done running?
* How does `uv tool install` choose where to put a program on my `PATH`? I think if you just put, "By using XDG" or something in parentheses, that would probably answer my question.

---

_Review comment by @BurntSushi on `docs/tools.md`:55 on 2024-07-23 18:41_

From reading the above commands, the mental model that I'm building is that each tool environment has a name, and that `uv tool install {name}` is a way to reference that environment. Is that right?

---

_Review comment by @BurntSushi on `docs/tools.md`:67 on 2024-07-23 18:42_

Are these meant to be a demonstration of equivalent commands? Now I think I'm wondering how to choose whether to use `uvx` versus `uv tool install`.

---

_Review comment by @BurntSushi on `docs/tools.md`:89 on 2024-07-23 18:42_

Ah okay, this answers my question above about `PATH`.

---

_@BurntSushi reviewed on 2024-07-23 18:43_

Nice! Just left some questions that came up in my head while reading.

---
