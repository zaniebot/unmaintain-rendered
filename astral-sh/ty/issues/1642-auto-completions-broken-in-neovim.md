```yaml
number: 1642
title: Auto-completions broken in neovim
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - server
assignees: []
created_at: 2025-11-26T08:20:01Z
updated_at: 2025-11-26T12:28:50Z
url: https://github.com/astral-sh/ty/issues/1642
synced_at: 2026-01-12T15:54:25Z
```

# Auto-completions broken in neovim

---

_@sharkdp_

Since ~yesterday, auto-completions in neovim are broken for me. It looks like only the first character is being used for matching. As I type the second, third, … character, completions are re-requested (at least there is a short flicker), but they do not update to reflect the narrower search term:

<img width="513" height="244" alt="Image" src="https://github.com/user-attachments/assets/0ecbd5b8-8d1d-4ae3-9a51-c90494893f46" />

<img width="420" height="196" alt="Image" src="https://github.com/user-attachments/assets/33795457-ab33-42b2-8add-93a53e302817" />

If I start with a longer substring (`NotImple<CURSOR>`) and request completions for the first time, it narrows down the search results, but then also fails to narrow them further:

<img width="755" height="164" alt="Image" src="https://github.com/user-attachments/assets/6cfcc794-0a80-4e88-bc87-755a6fea2b67" />

I can not reproduce this in VSCode.

```
▶ ty --version                                              
ty ruff/0.14.6+57 (536425619 2025-11-25)

~                                                                                                                                           
▶ nvim --version
NVIM v0.11.5
Build type: RelWithDebInfo
LuaJIT 2.1.1763148144
Run "nvim -V1 -v" for more info
```

### Neovim config

<details>

<summary>Relevant parts of my configuration (nothing has changed in a long time)</summary>

```lua
local configs = require('lspconfig.configs')
configs.ty = {
  default_config = {
    name = "ty",
    cmd = { '/home/shark/.cargo-target/debug/ty', 'server' },
    filetypes = { 'python' },
    root_dir = function(fname)
      return require('lspconfig.util').root_pattern('pyproject.toml', 'knot.toml')(fname)
        or vim.fs.dirname(vim.fs.find('.git', { path = fname, upward = true })[1])
    end,
    single_file_support = true,
    settings = {
        ty = {
           experimental = {
               autoImport = true
           }
        }
    },
  },
}

lspconfig.ty.setup {}

-- Format on save, auto-completion
vim.api.nvim_create_autocmd('LspAttach', {
  group = vim.api.nvim_create_augroup('my.lsp', {}),
  callback = function(args)
    local client = assert(vim.lsp.get_client_by_id(args.data.client_id))
    if client:supports_method('textDocument/implementation') then
      -- Create a keymap for vim.lsp.buf.implementation ...
    end
    -- Enable auto-completion. Note: Use CTRL-Y to select an item. |complete_CTRL-Y|
    if client:supports_method('textDocument/completion') then
      -- Autoselect the first item but don't insert it.
      -- Allows quick use, just write something and enter to select the first one.
      vim.opt.completeopt = { "menu", "menuone", "noinsert" }

      -- Optional: trigger autocompletion on EVERY keypress. May be slow!
      local chars = {}; for i = 32, 126 do table.insert(chars, string.char(i)) end
      client.server_capabilities.completionProvider.triggerCharacters = chars
      vim.lsp.completion.enable(true, client.id, args.buf, {autotrigger = true})
    end
    -- Auto-format ("lint") on save.
    -- Usually not needed if server supports "textDocument/willSaveWaitUntil".
    if not client:supports_method('textDocument/willSaveWaitUntil')
        and client:supports_method('textDocument/formatting') then
      vim.api.nvim_create_autocmd('BufWritePre', {
        group = vim.api.nvim_create_augroup('my.lsp', {clear=false}),
        buffer = args.buf,
        callback = function()
          vim.lsp.buf.format({ bufnr = args.buf, id = client.id, timeout_ms = 1000 })
        end,
      })
    end
  end,
})
```

</details>

---

_Label `bug` added by @sharkdp on 2025-11-26 08:20_

---

_Label `server` added by @sharkdp on 2025-11-26 08:20_

---

_Comment by @AlexWaygood on 2025-11-26 08:21_

Cc. @burntsushi

---

_Comment by @MichaReiser on 2025-11-26 08:29_

I suspect this is because of the changes in https://github.com/astral-sh/ruff/pull/21576 where we only match from the last token's start up to `offset` and the reason why you can't repro this in VS Code is because VS Code keeps applying its own filtering?

---

_Added to milestone `Beta` by @MichaReiser on 2025-11-26 08:29_

---

_Assigned to @BurntSushi by @MichaReiser on 2025-11-26 08:29_

---

_Comment by @sharkdp on 2025-11-26 08:54_

> I suspect this is because of the changes in [astral-sh/ruff#21576](https://github.com/astral-sh/ruff/pull/21576) where we only match from the last token's start up to `offset` and the reason why you can't repro this in VS Code is because VS Code keeps applying its own filtering?

I now tried to bisect it, but couldn't. This seems to actually be a bug/regression in neovim! I can not reproduce this with 0.11.4. It only happens with 0.11.5. Closing this for now, sorry for the noise.

---

_Closed by @sharkdp on 2025-11-26 08:54_

---

_Comment by @BurntSushi on 2025-11-26 12:28_

FWIW, completions seem to be working fine for me in `nvim 0.11.5` (and `0.11.4`):

https://github.com/user-attachments/assets/7b528824-4af8-44e0-a5b4-559d84e5b741

They are also working for me in Helix.

---
