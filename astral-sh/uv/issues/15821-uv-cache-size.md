---
number: 15821
title: uv cache size
type: issue
state: closed
author: charlesnicholson
labels:
  - enhancement
assignees: []
created_at: 2025-09-12T21:23:58Z
updated_at: 2025-11-02T20:44:29Z
url: https://github.com/astral-sh/uv/issues/15821
synced_at: 2026-01-10T01:26:00Z
---

# uv cache size

---

_Issue opened by @charlesnicholson on 2025-09-12 21:23_

### Summary

Since uv's cache growth is unbounded, it would be helpful to have a built-in subcommand `uv cache size` or similar that can be parsed by scripts to decide whether or not to blow away the cache. Since `uv cache prune` already reports the cache size, maybe that logic would be easy to extract and offer up as a new subcommand?

### Example

_No response_

---

_Label `enhancement` added by @charlesnicholson on 2025-09-12 21:23_

---

_Comment by @charlesnicholson on 2025-09-14 01:13_

(if the cache size is itself stored in the cache, that would allow for near-instant size reporting, which would make the operation fast enough for us to run transparently in our script that launches uv for us)

---

_Referenced in [astral-sh/uv#5731](../../astral-sh/uv/issues/5731.md) on 2025-09-14 01:14_

---

_Comment by @samypr100 on 2025-09-14 01:45_

Would there be a limitation to using something like `du -hs $(uv cache dir)` for piping it to a script?

---

_Comment by @charlesnicholson on 2025-09-14 01:52_

Checking as quickly as possible if the cache has grown larger than a threshold is a building block to externally disallowing unbounded cache growth. If "prune the cache if it's larger than x GB" can't be done on every uv invocation, then there's no reason to have this because users could just manually prune their caches every now and then.

Additionally, getting the recursive size of a directory is generally extremely slow on Windows.

---

_Referenced in [astral-sh/uv#16032](../../astral-sh/uv/pulls/16032.md) on 2025-09-26 06:24_

---

_Closed by @charliermarsh on 2025-11-02 20:44_

---
