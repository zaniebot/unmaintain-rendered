---
number: 1313
title: "Allow `pip compile` to work with stdin"
type: issue
state: closed
author: mitsuhiko
labels:
  - compatibility
assignees: []
created_at: 2024-02-15T10:27:43Z
updated_at: 2024-03-12T23:56:07Z
url: https://github.com/astral-sh/uv/issues/1313
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow `pip compile` to work with stdin

---

_Issue opened by @mitsuhiko on 2024-02-15 10:27_

Currently you cannot compile a requirements.txt file from stdin. Only `/dev/stdin` works, but that's not portable. It would be nice if `-` could be used to imply stdin to match behavior with `pip-tools`.

---

_Label `compatibility` added by @zanieb on 2024-02-15 14:38_

---

_Referenced in [astral-sh/uv#1314](../../astral-sh/uv/pulls/1314.md) on 2024-02-15 15:54_

---

_Closed by @BurntSushi on 2024-02-15 16:25_

---

_Comment by @danielhollas on 2024-03-12 23:41_

It looks like this feature is currently not documented.

---

_Comment by @danielhollas on 2024-03-12 23:56_

Apologies, I did not realize there is a difference between `-h` and `--help`! :exploding_head: It is indeed documented in the full help output.

---
