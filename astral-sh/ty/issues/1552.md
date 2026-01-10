```yaml
number: 1552
title: Add LSP code action for inserting missing import statements
type: issue
state: closed
author: BurntSushi
labels:
  - server
assignees: []
created_at: 2025-11-14T12:57:32Z
updated_at: 2025-11-27T15:06:39Z
url: https://github.com/astral-sh/ty/issues/1552
synced_at: 2026-01-10T01:58:59Z
```

# Add LSP code action for inserting missing import statements

---

_Issue opened by @BurntSushi on 2025-11-14 12:57_

Here's a demonstration of this working in VS Code with pylance as the LSP server:

https://github.com/user-attachments/assets/eb25c9a2-6f4f-4c8f-8ced-f4fd6b478c83

In order to get that code action context menu to open, I used the `Control + .` keyboard shortcut.

This issue was forked off from #535.

-----

An initial implementation of this I think is mostly about doing the wiring on the LSP side. I think that picking the import can reuse the existing code for auto-import completions:

https://github.com/astral-sh/ruff/blob/d0314131fb470e0920d4935db97ae7b357821464/crates/ty_ide/src/all_symbols.rs#L7-L11

---

_Label `server` added by @BurntSushi on 2025-11-14 12:57_

---

_Label `completions` added by @BurntSushi on 2025-11-14 12:57_

---

_Comment by @AlexWaygood on 2025-11-14 13:01_

Is this the same as https://github.com/astral-sh/ty/issues/1549 ?

---

_Label `completions` removed by @BurntSushi on 2025-11-14 13:25_

---

_Comment by @BurntSushi on 2025-11-14 13:25_

Oh, I misread Micha's comment. Whoops. Let's go with this one, since I included a bit more info and a demo video.

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 13:25_

---

_Assigned to @Gankra by @Gankra on 2025-11-24 20:25_

---

_Closed by @Gankra on 2025-11-27 15:06_

---
