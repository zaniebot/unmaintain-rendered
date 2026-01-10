---
number: 13060
title: "`uv pip compile --format pylock.toml` option to preserve extras & dependency groups"
type: issue
state: open
author: paveldikov
labels:
  - enhancement
assignees: []
created_at: 2025-04-22T21:00:56Z
updated_at: 2025-07-17T21:00:29Z
url: https://github.com/astral-sh/uv/issues/13060
synced_at: 2026-01-10T01:25:28Z
---

# `uv pip compile --format pylock.toml` option to preserve extras & dependency groups

---

_Issue opened by @paveldikov on 2025-04-22 21:00_

### Summary

Right now `uv pip compile --format pylock.toml` emits a flattened list -- essentially equivalent to a TOML-ified `requirements.txt` with hashes & URLs.

Would be nice if it preserved the extras & dependency group-related metadata, so that one could do a partial install, much as with `uv.lock` files.

### Example

_No response_

---

_Label `enhancement` added by @paveldikov on 2025-04-22 21:00_

---

_Comment by @ketozhang on 2025-05-10 01:16_

+1, in the meantime note that the standard supports `pylock.*.toml`. You could put your dependency group name here.

https://peps.python.org/pep-0751/#file-name

---

_Comment by @gaborbernat on 2025-07-17 17:23_

I'll try to tackle this 😊 

---

_Referenced in [astral-sh/uv#14728](../../astral-sh/uv/pulls/14728.md) on 2025-07-18 15:21_

---
