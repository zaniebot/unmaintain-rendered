```yaml
number: 1560
title: Rename file refactor
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-14T17:35:12Z
updated_at: 2026-01-12T06:24:36Z
url: https://github.com/astral-sh/ty/issues/1560
synced_at: 2026-01-12T15:54:25Z
```

# Rename file refactor

---

_@MichaReiser_

Update import statements when a user renames a file

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 17:35_

---

_Label `server` added by @MichaReiser on 2025-11-14 17:35_

---

_Comment by @dhruvmanila on 2026-01-12 06:24_

I think this is the [`workspace/willRenameFiles` request](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_willRenameFiles) or [`workspace/didRenameFiles` notification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_didRenameFiles) depending on when we want to update those import statements (before or after the file rename).

---
