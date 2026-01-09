---
number: 22326
title: Why are Neovim editor settings of ruff and ty not aligned?
type: issue
state: closed
author: jamilraichouni
labels:
  - question
assignees: []
created_at: 2026-01-01T11:50:25Z
updated_at: 2026-01-01T15:43:55Z
url: https://github.com/astral-sh/ruff/issues/22326
synced_at: 2026-01-07T13:12:16-06:00
---

# Why are Neovim editor settings of ruff and ty not aligned?

---

_Issue opened by @jamilraichouni on 2026-01-01 11:50_

### Question

When I compare

<https://github.com/astral-sh/ruff/blob/main/docs/editors/settings.md>
and
<https://github.com/astral-sh/ty/blob/main/docs/reference/editor-settings.md>


I see (for Neovim 0.11.0 and later)

```lua
vim.lsp.config("ruff", {
    init_options = {
        settings = {
            showSyntaxErrors = true,
        }
    }
})
```

versus

```lua
vim.lsp.config("ty", {
    settings = {
        ty = {
            showSyntaxErrors = false,
        }
    }
})
```

Why is that?

For Neovim 0.11.0 and later I'd expect

```lua
vim.lsp.config("ruff", {
    settings = {
        showSyntaxErrors = false,
    }
})
```

respectively

```lua
vim.lsp.config("ty", {
    settings = {
        showSyntaxErrors = false,
    }
})
```

so that it looks the same.

All best,
Jamil

### Version

ruff 0.14.10
ty 0.0.8



---

_Label `question` added by @jamilraichouni on 2026-01-01 11:50_

---

_Comment by @MichaReiser on 2026-01-01 15:43_

This is because ty supports the newer workspaceConfiguration capability of the LSP specification whereas Ruff only supports `initializeOptions`.

---

_Closed by @MichaReiser on 2026-01-01 15:43_

---
