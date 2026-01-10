---
number: 21140
title: Process larger files first
type: issue
state: open
author: adamchainz
labels:
  - performance
assignees: []
created_at: 2025-10-30T14:35:35Z
updated_at: 2025-10-30T15:12:36Z
url: https://github.com/astral-sh/ruff/issues/21140
synced_at: 2026-01-10T01:23:02Z
---

# Process larger files first

---

_Issue opened by @adamchainz on 2025-10-30 14:35_

https://github.com/psf/black/issues/4771 describes ~20% speedup ofr Black by processing larger files first, so parallel workers pack the work better. After a quick look at Ruff, it seems that the same could work here, for example by sorting by size, descending, before `par_iter()` in `commands/check.rs`:

https://github.com/astral-sh/ruff/blob/1b0ee4677e216562033f8a2f9b006738734cb2b9/crates/ruff/src/commands/check.rs#L69

---

_Comment by @ntBre on 2025-10-30 15:08_

Ha, I was watching Anthony's video on this last night! We haven't actually read the files yet at this point in the code, so it could be slightly involved to implement this, but it could still be worth looking into.

---

_Label `performance` added by @ntBre on 2025-10-30 15:08_

---

_Comment by @MichaReiser on 2025-10-30 15:12_

This seems tricky because the file might also be cached, in which case checking it is literally free. 

---
