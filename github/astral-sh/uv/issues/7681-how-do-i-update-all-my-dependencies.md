---
number: 7681
title: How do i update all my dependencies ?
type: issue
state: closed
author: tim-x-y-z
labels: []
assignees: []
created_at: 2024-09-25T08:31:30Z
updated_at: 2024-09-25T09:00:01Z
url: https://github.com/astral-sh/uv/issues/7681
synced_at: 2026-01-07T13:12:17-06:00
---

# How do i update all my dependencies ?

---

_Issue opened by @tim-x-y-z on 2024-09-25 08:31_

Hi thanks for the great project! 

I'm coming from `poetry` where i can do a `poetry update` and that updates my lock files to all the latest versions allowed.
With `uv` i'm failing to do that in particular, i get a warning that one of my package has been "yanked" and it's quite frustrating it doesn't upgrade it automatically.

i understand there is `uv sync --upgrade-package ...` but this is not what i'm looking for. i want a command that updates all my dependencies without having me to know which one i want to update.

`uv sync` or `uv lock` doesn't update the lockfile. 
i'm on macos, with version `0.4.15` (and tried with `0.4.16`)


---

_Comment by @weihenglim on 2024-09-25 08:57_

> i understand there is uv sync --upgrade-package ... but this is not what i'm looking for. i want a command that updates all my dependencies without having me to know which one i want to update.

I think what you're looking for is `uv sync --upgrade`

---

_Comment by @tim-x-y-z on 2024-09-25 09:00_

Oh amazing - exactly, thank you very much! 

---

_Closed by @tim-x-y-z on 2024-09-25 09:00_

---

_Referenced in [astral-sh/uv#8880](../../astral-sh/uv/issues/8880.md) on 2024-11-07 12:49_

---
