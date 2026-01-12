```yaml
number: 14959
title: LSP ruff messages about logs, but the LspLog in nvim is empty, is there somewhere else to look?
type: issue
state: closed
author: kaddkaka
labels:
  - question
  - server
assignees: []
created_at: 2024-12-13T13:55:54Z
updated_at: 2025-01-08T12:48:02Z
url: https://github.com/astral-sh/ruff/issues/14959
synced_at: 2026-01-12T15:54:54Z
```

# LSP ruff messages about logs, but the LspLog in nvim is empty, is there somewhere else to look?

---

_@kaddkaka_

In Neovim when I open a python file in neovim with ruff server configured (in same as in other repo where don't see any error messages) I get this message:

`LSP[ruff] Error while resolving settings from workspace /<my_repo>. Please refer to the logs for more details.`

However the `:LspLog` in Neovim is empty. Is there some other log file where I can find more details?

## env
- neovim 0.10.2
- ruff 0.7.0
- lsp_config is empty/default:
```lua
local lsp = require('lspconfig')
lsp.ruff.setup {}
```

## similar issues:
I found a similar issue where the solution seemed to be to activate notifications in VScode, but this is for another editor that doesn't have the ruff server configuration.



---

_Label `server` added by @AlexWaygood on 2024-12-13 14:02_

---

_Comment by @dhruvmanila on 2024-12-13 16:11_

Sorry, I've been meaning to tackle this issue in general soon. 

There are similar settings for other editors as well. So, for Neovim you'd do the following which is mentioned as the last snippet on https://docs.astral.sh/ruff/editors/setup/#neovim:

```lua
require('lspconfig').ruff.setup {
  trace = 'messages',
  init_options = {
    settings = {
      logLevel = 'debug',
    }
  }
}
```

---

_Label `question` added by @dhruvmanila on 2024-12-13 16:11_

---

_Closed by @dhruvmanila on 2025-01-08 12:48_

---

_Closed by @dhruvmanila on 2025-01-08 12:48_

---
