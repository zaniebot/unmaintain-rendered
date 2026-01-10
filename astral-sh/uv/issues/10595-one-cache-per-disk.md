---
number: 10595
title: One cache per disk
type: issue
state: open
author: EtienneT
labels: []
assignees: []
created_at: 2025-01-14T13:43:35Z
updated_at: 2025-12-08T18:37:19Z
url: https://github.com/astral-sh/uv/issues/10595
synced_at: 2026-01-10T01:24:55Z
---

# One cache per disk

---

_Issue opened by @EtienneT on 2025-01-14 13:43_

Like pnpm, uv should have one cache folder per disk.  This way, uv could always use simlinks to link packages.

---

_Referenced in [astral-sh/uv#11721](../../astral-sh/uv/issues/11721.md) on 2025-02-23 09:21_

---

_Comment by @rsyring on 2025-08-30 14:33_

uv already uses techniques to limit disk usage by venvs sharing the same dependencies:

- https://github.com/astral-sh/uv/issues/11504
- https://docs.astral.sh/uv/reference/settings/#link-mode

---
