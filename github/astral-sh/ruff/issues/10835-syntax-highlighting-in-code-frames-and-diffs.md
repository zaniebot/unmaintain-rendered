---
number: 10835
title: Syntax highlighting in code frames and diffs
type: issue
state: open
author: MichaReiser
labels:
  - cli
  - wish
  - diagnostics
assignees: []
created_at: 2024-04-08T12:02:58Z
updated_at: 2025-09-26T18:11:00Z
url: https://github.com/astral-sh/ruff/issues/10835
synced_at: 2026-01-07T13:12:15-06:00
---

# Syntax highlighting in code frames and diffs

---

_Issue opened by @MichaReiser on 2024-04-08 12:02_

We should consider highlighting code frames and diffs. 

I think a good first step would be to implement semantic highlighting into `ruff server` that, given a string, emits a description of the highlighting. The codeframe and diff rendering can then reuse the same infrastructure to implement.

This is related to https://github.com/astral-sh/ruff-lsp/issues/237

 

---

_Label `cli` added by @MichaReiser on 2024-04-08 12:02_

---

_Label `wish` added by @MichaReiser on 2024-04-08 12:02_

---

_Comment by @Zander-1024 on 2024-06-06 08:24_

This is really important. I hope this feature can be implemented soon.

---

_Comment by @SnirShechter on 2024-06-13 12:00_

I'd very much like to see that.

Currently when using VSCode, you must use the Python extension for syntax highlighting so ruff can't completely replace it and they be used together (which kind-of misses the point of being fast when you must use another extension that's slow).

---

_Comment by @danny-huang-openfind on 2024-09-05 00:31_

While this feature is also important to me, I believe a linter shouldn't be concerned with it. Ruff should focus on "making every kind of linter work within its scope," rather than "trying to solve everything."

---

_Comment by @oramirite on 2024-11-23 22:59_

I think ruff's leap into automatic formatting - and doing a damn good job of it - kinda broke that seal already. I think the desire to see this feature stems from that. Completing the "triad" of core tools that would allow for ruff to be a complete replacement for the multiple plugins we currently need, would be a Godsend.

I'd very much enjoy having centralized configuration if these elements in ruff so I can fully replace the Intelligence stuff (it can be very slow).

---

_Label `diagnostics` added by @MichaReiser on 2025-01-21 13:44_

---

_Comment by @rivershah on 2025-04-09 16:22_

+1 . Essential feature for wide adoption

---

_Comment by @showgood163 on 2025-09-26 16:58_

Will ruff support syntax highlighting in the foreseeable future?

---

_Comment by @MichaReiser on 2025-09-26 17:32_

Syntax highlighting in diagnostics isn't something on our current roadmap but would certainly be nice to have. [ty](https://github.com/astral-sh/ty) supports semantic syntax highlighting. 

---

_Comment by @showgood163 on 2025-09-26 18:11_

Thanks for pointing out ty.

I see that ty is experimental. Is there any advice on what's not working and workarounds?

---
