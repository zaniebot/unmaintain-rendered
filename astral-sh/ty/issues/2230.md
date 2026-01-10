---
number: 2230
title: Add / document support for Claude Code LSP integration
type: issue
state: open
author: adamtheturtle
labels:
  - question
assignees: []
created_at: 2025-12-26T17:05:23Z
updated_at: 2026-01-08T13:40:35Z
url: https://github.com/astral-sh/ty/issues/2230
synced_at: 2026-01-10T01:51:14Z
---

# Add / document support for Claude Code LSP integration

---

_Issue opened by @adamtheturtle on 2025-12-26 17:05_

I see `pyright-lsp` on https://code.claude.com/docs/en/plugins-reference#lsp-servers and would be happy for Claude to use ty.

---

_Comment by @MichaReiser on 2025-12-26 17:40_

I don't know what we need to do to get on claude's documentation. But you should definitely be able to use ty with claude. 

Something like this should work (untested):


```json
{
  "python": {
    "command": "uvx ty@latest",
    "args": ["servern"],
    "extensionToLanguage": {
      ".py": "python",
	  ".pyi": "python"
    }
  }
}
```

You can find all other supported options here https://docs.astral.sh/ty/reference/editor-settings/

---

_Label `question` added by @MichaReiser on 2025-12-26 17:40_

---

_Comment by @adamtheturtle on 2025-12-26 17:43_

Thank you, I think that it would be good for that to be in ty's documentation.

---

_Comment by @ilepn on 2026-01-08 13:40_

@MichaReiser I've created a reference plugin implementation: https://github.com/ilepn/ty-lsp-claude-code

Configuration uses `uvx ty@latest server` as you suggested. Plugin structure follows Claude Code's LSP plugin spec.

---
