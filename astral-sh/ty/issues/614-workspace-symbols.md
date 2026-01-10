```yaml
number: 614
title: Workspace symbols
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-06-09T07:17:57Z
updated_at: 2025-07-31T19:20:58Z
url: https://github.com/astral-sh/ty/issues/614
synced_at: 2026-01-10T02:06:24Z
```

# Workspace symbols

---

_Issue opened by @MichaReiser on 2025-06-09 07:17_

Support the workspace symbol request. It is used to find a symbol by name. https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_symbol

---

_Added to milestone `Beta` by @MichaReiser on 2025-06-09 07:17_

---

_Label `server` added by @MichaReiser on 2025-06-09 07:17_

---

_Comment by @MichaReiser on 2025-06-09 07:19_

Cc: @BurntSushi 

---

_Comment by @dhruvmanila on 2025-06-09 08:22_

Maybe we should look into [document symbol](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_documentSymbol) first which is for a specific document (file)?

---

_Comment by @MichaReiser on 2025-06-09 14:39_

Possibly, seems less useful though 

---

_Comment by @dhruvmanila on 2025-06-10 02:20_

Personally, I find it equally useful with workspace symbols. I use this when I want to navigate to a different part of the file and using document symbols is quicker than searching for that symbol, function, etc. It would also be quicker than workspace symbols. And, I think the "Search buffer symbol" in Zed is probably also using this.

<details><summary>Zed's "outline: toggle" screenshot:</summary>
<p>

<img width="577" alt="Image" src="https://github.com/user-attachments/assets/c3cd60fa-18a5-4626-af5b-665819a8ae8b" />

</p>
</details> 

---

_Comment by @MichaReiser on 2025-07-31 19:20_

An initial version of this is now implemented, see https://github.com/astral-sh/ruff/pull/19521

---

_Closed by @MichaReiser on 2025-07-31 19:20_

---
