---
number: 2233
title: EXE003 should allow a pytest hashbang?
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-26T22:04:43Z
updated_at: 2023-01-26T22:32:24Z
url: https://github.com/astral-sh/ruff/issues/2233
synced_at: 2026-01-07T13:12:14-06:00
---

# EXE003 should allow a pytest hashbang?

---

_Issue opened by @spaceone on 2023-01-26 22:04_

`EXE003` complains about the hashbang `#!/usr/bin/pytest-3` which is valid.

---

_Comment by @charliermarsh on 2023-01-26 22:05_

Can you link to any relevant docs?

---

_Comment by @charliermarsh on 2023-01-26 22:05_

Happy to change it, would just be nice to have something to link to.

---

_Comment by @spaceone on 2023-01-26 22:14_

what exactly do you need? the docs at https://docs.pytest.org/ don't mention this or the command line arguments.
https://docs.pytest.org/en/7.2.x/how-to/usage.html is about the invocation but this is not listed.
the paths may also vary between operating systems.

---

_Comment by @charliermarsh on 2023-01-26 22:28_

Yeah not sure what I was expecting. But I'll fix this anyway.

---

_Referenced in [astral-sh/ruff#2237](../../astral-sh/ruff/pulls/2237.md) on 2023-01-26 22:32_

---

_Closed by @charliermarsh on 2023-01-26 22:32_

---
