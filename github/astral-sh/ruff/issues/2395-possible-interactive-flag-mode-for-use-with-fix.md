---
number: 2395
title: "Possible `--interactive` flag/mode for use with `--fix`?"
type: issue
state: closed
author: ngnpope
labels:
  - cli
assignees: []
created_at: 2023-01-31T13:37:18Z
updated_at: 2023-05-10T20:45:40Z
url: https://github.com/astral-sh/ruff/issues/2395
synced_at: 2026-01-07T13:12:14-06:00
---

# Possible `--interactive` flag/mode for use with `--fix`?

---

_Issue opened by @ngnpope on 2023-01-31 13:37_

This is more just throwing an idea in the pot for consideration down the line.

What about an interactive mode for applying autofix patches à la `git add --patch`?

I saw #2390 and wondered whether this could be useful to skip over unsafe or incorrect changes...
Right now it's apply fixes to all files or none unless manually crafting the list of files on the command line.


---

_Label `cli` added by @charliermarsh on 2023-02-03 18:23_

---

_Comment by @charliermarsh on 2023-03-13 21:24_

\cc @MichaReiser regarding "review mode"

---

_Referenced in [astral-sh/ruff#4361](../../astral-sh/ruff/issues/4361.md) on 2023-05-10 20:38_

---

_Comment by @charliermarsh on 2023-05-10 20:45_

Consolidating into https://github.com/charliermarsh/ruff/issues/4361.

---

_Closed by @charliermarsh on 2023-05-10 20:45_

---
