---
number: 2180
title: Find references runs out of memory
type: issue
state: open
author: MichaReiser
labels:
  - server
  - fatal
assignees: []
created_at: 2025-12-23T09:01:30Z
updated_at: 2025-12-31T16:35:59Z
url: https://github.com/astral-sh/ty/issues/2180
synced_at: 2026-01-10T01:51:14Z
---

# Find references runs out of memory

---

_Issue opened by @MichaReiser on 2025-12-23 09:01_

### Summary

> With the language server, I accidentally did a "show all references" on logging and it kept going until it ran out of memory. I'm new to LSPs so I don't know if it's the fault of the vim plugin I'm using or ty for not stopping when the list length became unreasonable

Originally reported [on Discord](https://discord.com/channels/1039017663004942429/1279201882337705994/1452934695871582208)

### Version

_No response_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-23 09:01_

---

_Label `server` added by @MichaReiser on 2025-12-23 09:01_

---

_Label `fatal` added by @MichaReiser on 2025-12-23 09:01_

---

_Comment by @salty-horse on 2025-12-23 09:16_

I reported this. This was the built-in `logging` module, and I was using the [lsp](https://github.com/yegappan/lsp) plugin for Vim.

The project itself is small, with only 190 imports of `logging`, so I think it was scanning all the package dependencies.

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:35_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:35_

---
