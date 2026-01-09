---
number: 13031
title: "Accept `pylock.toml` as a constraint file"
type: issue
state: open
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
created_at: 2025-04-21T23:11:23Z
updated_at: 2025-06-06T14:18:18Z
url: https://github.com/astral-sh/uv/issues/13031
synced_at: 2026-01-07T13:12:18-06:00
---

# Accept `pylock.toml` as a constraint file

---

_Issue opened by @charliermarsh on 2025-04-21 23:11_

E.g., `uv pip install flask -c pylock.toml`.

---

_Label `enhancement` added by @charliermarsh on 2025-04-21 23:11_

---

_Label `cli` added by @charliermarsh on 2025-04-21 23:11_

---

_Comment by @paveldikov on 2025-04-22 21:05_

This would be terrifically useful since currently we use constraints files as the next best thing. Being able to just flip the `--format` & filename of the lockfile would allow for a really smooth migration, without immediately forcing `sync` semantics onto a large population of existing projects. Hoping `pip` ends up supporting this pattern, too.

---

_Comment by @itcarroll on 2025-06-06 14:18_

Could this, or would this also constrain `uv lock`? I'm not clear right now on how to approach a project that comes with only a `pylock.toml`. I think I still need a `uv.lock` but I'm not sure how to generate it while respecting `pylock.toml`.

---

_Referenced in [astral-sh/uv#15947](../../astral-sh/uv/issues/15947.md) on 2025-09-19 13:23_

---
