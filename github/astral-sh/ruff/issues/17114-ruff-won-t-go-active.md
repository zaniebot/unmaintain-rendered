---
number: 17114
title: "Ruff won't go active"
type: issue
state: closed
author: LeoAdavirat
labels:
  - question
assignees: []
created_at: 2025-04-01T11:21:30Z
updated_at: 2025-04-28T08:33:35Z
url: https://github.com/astral-sh/ruff/issues/17114
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff won't go active

---

_Issue opened by @LeoAdavirat on 2025-04-01 11:21_

### Summary

This is my init.lua, but Ruff simply won't go active.

```lua
-- You can add your own plugins here or in other files in this directory!
--  I promise not to create any merge conflicts in this directory :)
--
-- See the kickstart.nvim README for more information

require('lazy').setup {
  {
    'nvimtools/none-ls.nvim',
    dependencies = { 'nvimtools/none-ls-extras.nvim' },
    config = function() end,
  },
}

require('lspconfig').ruff.setup {
  on_attach = function(client, bufnr)
    vim.diagnostic.on_attach(client, bufnr)
  end,
  settings = {
    ruff = {
      enable = true,
    },
  },
}
use {
  'nvimtools/none-ls.nvim',
  requires = { 'nvimtools/none-ls-extras.nvim' },
}
local none_lsp = require 'none-ls'

none_lsp.setup {
  sources = {
    none_lsp.builtins.diagnostics.ruff,
    none_lsp.builtins.formatting.ruff,
  },
}
```

### Version

_No response_

---

_Comment by @MichaReiser on 2025-04-01 12:34_

Are there any vim logs that you could share (maybe try setting https://docs.astral.sh/ruff/editors/settings/#loglevel)? Does running `ruff check` on the same file report an error? 

---

_Label `question` added by @dhruvmanila on 2025-04-03 14:27_

---

_Comment by @dhruvmanila on 2025-04-03 14:28_

I don't see any `vim.diagnostic.on_attach` method. Can you clarify what that is?

---

_Closed by @MichaReiser on 2025-04-28 08:33_

---
