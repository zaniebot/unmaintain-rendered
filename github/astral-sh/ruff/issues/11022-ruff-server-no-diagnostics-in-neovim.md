---
number: 11022
title: "`ruff server`: No diagnostics in Neovim"
type: issue
state: closed
author: MithicSpirit
labels:
  - bug
  - server
assignees: []
created_at: 2024-04-19T02:14:34Z
updated_at: 2024-04-29T14:41:20Z
url: https://github.com/astral-sh/ruff/issues/11022
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server`: No diagnostics in Neovim

---

_Issue opened by @MithicSpirit on 2024-04-19 02:14_

## Description

When using `ruff server`, there appear to be no diagnostics in Neovim (although other features work as expected). This issue is not reflected in `ruff_lsp`.

## Minimal example

I have been able to reproduce the following in Docker (Arch and Alpine images) after installing neovim, git, and ruff/ruff_lsp (through pipx).

1. Create the file below as `repro.lua` (basic lazy.nvim MWE template + nvim-lspconfig setup)
2. Run `nvim -u repro.lua` to install plugins, then quit after they are installed
3. Run `nvim -u repro.lua test.py` and try typing some code that ruff would complain about (no diagnostics; unexpected)

As a sanity check, you can also try the following:

4. With the file still open write some code that ruff would have a fix for (e.g.: just `import foo`), use `<Space>ca` to open the code actions menu, and select option `1` to trigger the fix (code is fixed; expected)
5. Edit `repro.lua` to change the last line to `require('lspconfig').ruff_lsp.setup {}` (basically just add the `_lsp`)
6. Repeat step 3 (diagnostics appear; expected)

```lua
-- DO NOT change the paths and don't remove the colorscheme
local root = vim.fn.fnamemodify("./.repro", ":p")

-- set stdpaths to use .repro
for _, name in ipairs({ "config", "data", "state", "cache" }) do
  vim.env[("XDG_%s_HOME"):format(name:upper())] = root .. "/" .. name
end

-- bootstrap lazy
local lazypath = root .. "/plugins/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({ "git", "clone", "--filter=blob:none", "https://github.com/folke/lazy.nvim.git", lazypath, })
end
vim.opt.runtimepath:prepend(lazypath)

-- install plugins
local plugins = {
  "folke/tokyonight.nvim",
  -- add any other plugins here
  "neovim/nvim-lspconfig"
}
require("lazy").setup(plugins, {
  root = root .. "/plugins",
})

vim.cmd.colorscheme("tokyonight")
-- add anything else here
local lspconfig = require('lspconfig')

-- Global mappings.
-- See `:help vim.diagnostic.*` for documentation on any of the below functions
vim.keymap.set('n', '<space>e', vim.diagnostic.open_float)
vim.keymap.set('n', '[d', vim.diagnostic.goto_prev)
vim.keymap.set('n', ']d', vim.diagnostic.goto_next)
vim.keymap.set('n', '<space>q', vim.diagnostic.setloclist)

-- Use LspAttach autocommand to only map the following keys
-- after the language server attaches to the current buffer
vim.api.nvim_create_autocmd('LspAttach', {
  group = vim.api.nvim_create_augroup('UserLspConfig', {}),
  callback = function(ev)
    -- Enable completion triggered by <c-x><c-o>
    vim.bo[ev.buf].omnifunc = 'v:lua.vim.lsp.omnifunc'

    -- Buffer local mappings.
    -- See `:help vim.lsp.*` for documentation on any of the below functions
    local opts = { buffer = ev.buf }
    vim.keymap.set('n', 'gD', vim.lsp.buf.declaration, opts)
    vim.keymap.set('n', 'gd', vim.lsp.buf.definition, opts)
    vim.keymap.set('n', 'K', vim.lsp.buf.hover, opts)
    vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, opts)
    vim.keymap.set('n', '<C-k>', vim.lsp.buf.signature_help, opts)
    vim.keymap.set('n', '<space>wa', vim.lsp.buf.add_workspace_folder, opts)
    vim.keymap.set('n', '<space>wr', vim.lsp.buf.remove_workspace_folder, opts)
    vim.keymap.set('n', '<space>wl', function()
      print(vim.inspect(vim.lsp.buf.list_workspace_folders()))
    end, opts)
    vim.keymap.set('n', '<space>D', vim.lsp.buf.type_definition, opts)
    vim.keymap.set('n', '<space>rn', vim.lsp.buf.rename, opts)
    vim.keymap.set({ 'n', 'v' }, '<space>ca', vim.lsp.buf.code_action, opts)
    vim.keymap.set('n', 'gr', vim.lsp.buf.references, opts)
    vim.keymap.set('n', '<space>f', function()
      vim.lsp.buf.format { async = true }
    end, opts)
  end,
})
require('lspconfig').ruff.setup {}
```


---

_Label `bug` added by @snowsignal on 2024-04-19 02:15_

---

_Label `server` added by @snowsignal on 2024-04-19 02:15_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-19 02:15_

---

_Comment by @dhruvmanila on 2024-04-19 09:17_

I think I need to look into this as it might be the new parser which is causing it.

---

_Comment by @dhruvmanila on 2024-04-19 12:24_

@MithicSpirit Is it like on any code that the diagnostics aren't visible or is it a specific one? Does it contain syntax errors? Is it possible for you to share a small snippet from it? Are you seeing any error messages while you type?

---

_Comment by @MithicSpirit on 2024-04-19 14:24_

@dhruvmanila Seems to be that all diagnostics are not visible on all code. The code I've been using for testing is just the one-liner `import foo` (which should trigger F401). There are no error messages in the diagnostics, but there are some messages in `:LspLog`; the following seem relevant.
```
[ERROR][2024-04-19 14:11:40] .../vim/lsp/rpc.lua:734    "rpc"   "/root/.local/bin/ruff" "stderr"        "   0.001722s DEBUG ruff_server::server No workspace(s) were provided during initialization. Using the current working directory as a default workspace...\n"
[ERROR][2024-04-19 14:11:40] .../vim/lsp/rpc.lua:734    "rpc"   "/root/.local/bin/ruff" "stderr"        "   0.174506s WARN ruff_server::server LSP client does not support dynamic capability registration - automatic configuration reloading will not be available.\n"
[ERROR][2024-04-19 14:12:40] .../vim/lsp/rpc.lua:734    "rpc"   "/root/.local/bin/ruff" "stderr"        "  59.445650s INFO ruff_server::session::workspace::ruff_settings No ruff settings file (pyproject.toml/ruff.toml/.ruff.toml) found for /test.py - falling back to default configuration\n"
```
As a side note, there are a lot of messages being printed to stderr that are not error messages (mostly "file changed" notifications), but all of these get picked up as `[ERROR]`s by Neovim. Maybe these should be logged some other way? Not sure how logging is usually done for LSPs.

EDIT: just tried doing this inside another project (tried with https://github.com/pandas-dev/pandas because why not) and the first and third messages ("No workspaces" and "No ruff settings file") did not appear in `:LspLog`. The "dynamic capability registration" message still appeared, however.

---

_Comment by @bluthej on 2024-04-20 07:36_

I am experiencing a similar issue with Helix (v24.3), for example:
- formatting on save works
- I can trigger the import sorting code action

but the diagnostics don' show up and when I look at the log I have a lot of these messages:
```
2024-04-20T09:32:13.041 helix_lsp::transport [ERROR] ruff err <- "┘\n"
2024-04-20T09:32:13.190 helix_lsp::transport [ERROR] ruff err <- "┐ruff_server::server::api::notifications::did_change::run{file=file:///home/joffrey/repos/numerical-schemes/main.py}\n"
2024-04-20T09:32:13.190 helix_lsp::transport [ERROR] ruff err <- "┘\n"
2024-04-20T09:32:14.840 helix_lsp::transport [ERROR] ruff err <- "┐ruff_formatter::printer::Printer::print{}\n"
2024-04-20T09:32:14.840 helix_lsp::transport [ERROR] ruff err <- "┘\n"
```
I have no idea where all those "┘" are coming from! None of this shows up with `ruff-lsp`.

---

_Comment by @snowsignal on 2024-04-20 15:16_

@bluthej I think the "┘" are a side-effect of how we currently log things. Right now, the LSP logs to `stderr` instead of using the LSP protocol, which is why they appear as `[ERROR]`s instead of their proper warning level. This will be fixed soon.

I'm currently investigating why diagnostics don't appear in Neovim, and my guess is that the problems you're experiencing in Helix are probably related.

---

_Comment by @MithicSpirit on 2024-04-20 21:55_

As per the discussion with @snowsignal on the Discord, the issue appears to be with pull diagnostics. Building the latest Neovim commit from source, which has support therefor, fixes the issue. I think it would be appropriate to document the requirement for pull diagnostics and update the Neovim setup guide to mention 0.10 (not yet released) is required for diagnostics.

---

_Comment by @snowsignal on 2024-04-20 22:00_

@bluthej This is also why Helix diagnostics don't work yet - pull diagnostics aren't supported by the Helix LSP client yet. There was [a recent pull request to implement them](https://github.com/helix-editor/helix/pull/7900), but unfortunately it's been abandoned.

---

_Referenced in [astral-sh/ruff#11059](../../astral-sh/ruff/issues/11059.md) on 2024-04-20 22:20_

---

_Comment by @snowsignal on 2024-04-20 22:20_

I've created a new issue to track implementation of `publishDiagnostics`.

---

_Comment by @dhruvmanila on 2024-04-21 05:38_

Oh wow, good find. I'm always on Neovim nightly which is why I did not see this behavior. Thanks for debugging it! I think we could technically create an integration test, at least for Neovim, to check that certain features work on the latest (two?) stable version.

---

_Referenced in [LazyVim/LazyVim#3037](../../LazyVim/LazyVim/pulls/3037.md) on 2024-04-24 15:30_

---

_Comment by @snowsignal on 2024-04-29 14:41_

@MithicSpirit This was fixed in `v0.4.2`. It should also be fixed for Helix! Let me know if you run into any more issues.

---

_Closed by @snowsignal on 2024-04-29 14:41_

---
