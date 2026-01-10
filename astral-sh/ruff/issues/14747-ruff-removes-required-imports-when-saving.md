```yaml
number: 14747
title: Ruff removes required imports when saving
type: issue
state: closed
author: vgoklani
labels:
  - needs-mre
assignees: []
created_at: 2024-12-03T02:07:16Z
updated_at: 2025-01-15T07:42:44Z
url: https://github.com/astral-sh/ruff/issues/14747
synced_at: 2026-01-10T11:09:56Z
```

# Ruff removes required imports when saving

---

_Issue opened by @vgoklani on 2024-12-03 02:07_

Hey there,

I've noticed some strange behavior when saving code, as some of the import statements get removed even though they are referenced in the actual code.

I am using Sublime Text 4180 + LSP-ruff v2.03, and this is my ruff.toml:

```
unfixable = ["F401"]

[lint.isort]
split-on-trailing-comma = false

[format]
skip-magic-trailing-comma = true
```

As a concrete example, if we take the code from here:

https://gist.githubusercontent.com/N8python/2431ab577d1de60e46180fa43a743c61/raw/453e677891eeb229ec4f05033ab6e0a4a8957aa3/MORE.py

and paste it into a new file and then save, everything above the "# Copyright © 2023-2024 Apple Inc." gets dropped after saving. Some of those imports are required.

I then press `undo` *twice*, and the imports get magically added back, but the code is still linted. This behavior of having to press undo to fix things is unexpected. Moreover, pressing undo does not take the code back to the original pasted code (which is counter-intuitive). 

There is something funny going on here :) I hope this makes sense. Thanks!


---

_Renamed from "Ruff is breaking my code" to "Ruff removed required imports when saving" by @vgoklani on 2024-12-03 02:07_

---

_Renamed from "Ruff removed required imports when saving" to "Ruff removes required imports when saving" by @vgoklani on 2024-12-03 02:07_

---

_Comment by @dhruvmanila on 2024-12-03 04:00_

Playground link for the same: https://play.ruff.rs/f6c9ed84-96a5-473b-94e6-10bc2b4e0b94

Can you specify the project structure? Like, where is the Python file and where's the Ruff config file (`ruff.toml`) ?

---

_Label `needs-mre` added by @MichaReiser on 2024-12-03 07:22_

---

_Comment by @MichaReiser on 2025-01-15 07:42_

I'll lose this as this issue isn't actionable without an MRE. Feel free to re-open or comment when you get around and have time to provide an MRE

---

_Closed by @MichaReiser on 2025-01-15 07:42_

---
