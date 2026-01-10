```yaml
number: 882
title: "LSP: SelectionRangeProvider"
type: issue
state: closed
author: guybuk
labels:
  - feature
  - server
assignees: []
created_at: 2025-07-24T10:00:19Z
updated_at: 2025-07-31T19:16:17Z
url: https://github.com/astral-sh/ty/issues/882
synced_at: 2026-01-10T02:06:24Z
```

# LSP: SelectionRangeProvider

---

_Issue opened by @guybuk on 2025-07-24 10:00_

Hello, 
Thank you for this project.

I'm an avid user of vscode's smart select (`editor.action.smartSelect.expand`, `editor.action.smartSelect.shrink`). Without a language server, this feature doesn't recognize pythonic syntax like function definitions, loops, context managers, etc. 

If I understand correctly, the language server (ty) would need to conform with the [SelectionRangeProvider API](https://code.visualstudio.com/api/references/vscode-api#SelectionRangeProvider) for this to work.

If this could be added in the future, it would be greatly appreciated.

Thanks!

---

_Comment by @MichaReiser on 2025-07-24 11:53_

I'll move this to the ty repository as it isn't specific to VS Code

---

_Renamed from "Feature Request: SelectionRangeProvider" to "LSP: SelectionRangeProvider" by @MichaReiser on 2025-07-24 11:53_

---

_Label `feature` added by @MichaReiser on 2025-07-24 11:53_

---

_Label `server` added by @MichaReiser on 2025-07-24 11:53_

---

_Comment by @MichaReiser on 2025-07-31 19:16_

This is now implemented, see https://github.com/astral-sh/ruff/pull/19567

---

_Closed by @MichaReiser on 2025-07-31 19:16_

---
