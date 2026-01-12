```yaml
number: 775
title: Return CompletionItemKind for completions
type: issue
state: closed
author: JaRoSchm
labels:
  - server
  - completions
assignees: []
created_at: 2025-07-08T06:26:35Z
updated_at: 2025-10-03T12:19:28Z
url: https://github.com/astral-sh/ty/issues/775
synced_at: 2026-01-12T15:54:24Z
```

# Return CompletionItemKind for completions

---

_@JaRoSchm_

Hi, most language servers also return the kind (function, module, variable, ...) for each suggested completion, which is used by many editors to display small icons next to each item. If I understand the specification correctly, this is the [completionItemKind](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemKind) of each [completionItem](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItem). I think this would be a good addition to ty too.

Thank you for your great work!

---

_Label `server` added by @MichaReiser on 2025-07-08 06:28_

---

_Comment by @MichaReiser on 2025-07-08 06:29_

Yes, we do want to support this. We've just been focusing on the more basic completions for now but this is something we'll work on soon.

---

_Assigned to @BurntSushi by @MichaReiser on 2025-07-08 06:30_

---

_Closed by @BurntSushi on 2025-07-09 16:03_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---
