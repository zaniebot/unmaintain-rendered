---
number: 11259
title: "Code action breaks when using both `ruff_lsp` and `ruff` on unused import"
type: issue
state: closed
author: xl4624
labels:
  - server
assignees: []
created_at: 2024-05-03T04:18:51Z
updated_at: 2024-05-25T15:08:20Z
url: https://github.com/astral-sh/ruff/issues/11259
synced_at: 2026-01-07T13:12:15-06:00
---

# Code action breaks when using both `ruff_lsp` and `ruff` on unused import

---

_Issue opened by @xl4624 on 2024-05-03 04:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
"missing field 'range'", "deserialize diagnostic data", "KeyError: 'location'"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When I run neovim with this config:
```lua
local on_windows = vim.loop.os_uname().version:match 'Windows'

local function join_paths(...)
  local path_sep = on_windows and '\\' or '/'
  local result = table.concat({ ... }, path_sep)
  return result
end

vim.cmd [[set runtimepath=$VIMRUNTIME]]

local temp_dir = vim.loop.os_getenv 'TEMP' or '/tmp'

vim.cmd('set packpath=' .. join_paths(temp_dir, 'nvim', 'site'))

local package_root = join_paths(temp_dir, 'nvim', 'site', 'pack')
local lspconfig_path = join_paths(package_root, 'test', 'start', 'nvim-lspconfig')

if vim.fn.isdirectory(lspconfig_path) ~= 1 then
  vim.fn.system { 'git', 'clone', 'https://github.com/neovim/nvim-lspconfig', lspconfig_path }
end

vim.lsp.set_log_level 'trace'
require('vim.lsp.log').set_format_func(vim.inspect)
local nvim_lsp = require 'lspconfig'
local on_attach = function(_, bufnr)
  local function buf_set_option(...)
    vim.api.nvim_buf_set_option(bufnr, ...)
  end

  buf_set_option('omnifunc', 'v:lua.vim.lsp.omnifunc')

  -- Mappings.
  local opts = { buffer = bufnr, noremap = true, silent = true }
  vim.keymap.set('n', 'gD', vim.lsp.buf.declaration, opts)
  vim.keymap.set('n', 'gd', vim.lsp.buf.definition, opts)
  vim.keymap.set('n', 'K', vim.lsp.buf.hover, opts)
  vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, opts)
  vim.keymap.set('n', '<C-k>', vim.lsp.buf.signature_help, opts)
  vim.keymap.set('n', '<space>ca', vim.lsp.buf.code_action, opts)
  vim.keymap.set('n', '<space>wa', vim.lsp.buf.add_workspace_folder, opts)
  vim.keymap.set('n', '<space>wr', vim.lsp.buf.remove_workspace_folder, opts)
  vim.keymap.set('n', '<space>wl', function()
    print(vim.inspect(vim.lsp.buf.list_workspace_folders()))
  end, opts)
  vim.keymap.set('n', '<space>D', vim.lsp.buf.type_definition, opts)
  vim.keymap.set('n', '<space>rn', vim.lsp.buf.rename, opts)
  vim.keymap.set('n', 'gr', vim.lsp.buf.references, opts)
  vim.keymap.set('n', '<space>e', vim.diagnostic.open_float, opts)
  vim.keymap.set('n', '[d', vim.diagnostic.goto_prev, opts)
  vim.keymap.set('n', ']d', vim.diagnostic.goto_next, opts)
  vim.keymap.set('n', '<space>q', vim.diagnostic.setloclist, opts)
end

-- LspInfo shows:
-- Client: ruff (id: 1, bufnr: [1])
-- 	filetypes:       python
-- 	autostart:       true
-- 	root directory:  Running in single file mode.
-- 	cmd:             /Users/xiaomin/.local/share/nvim/mason/bin/ruff server --preview
-- 
-- Client: ruff_lsp (id: 2, bufnr: [1])
-- 	filetypes:       python
-- 	autostart:       true
-- 	root directory:  Running in single file mode.
-- 	cmd:             /Users/xiaomin/.local/share/nvim/mason/bin/ruff-lsp
--
-- Configured servers list: ruff, ruff_lsp

nvim_lsp['ruff'].setup {
  cmd = { '/Users/xiaomin/.local/share/nvim/mason/bin/ruff', 'server', '--preview' },
  on_attach = on_attach,
}
nvim_lsp['ruff_lsp'].setup {
  cmd = { '/Users/xiaomin/.local/share/nvim/mason/bin/ruff-lsp' },
  on_attach = on_attach,
}

print [[You can find your log at $HOME/.cache/nvim/lsp.log. Please paste in a github issue under a details tag as described in the issue template.]]
```

And then wait a bit for ruff to load in, when I try `<space>ca` on this test file:
```python
from datetime import datetime
```
I get "No code actions available"

Here are the relevant logs:
```
[ERROR][2024-05-02 23:55:00] .../vim/lsp/rpc.lua:734	"rpc"	"/Users/xiaomin/.local/share/nvim/mason/bin/ruff"	"stderr"	"  62.696570s INFO ruff_server::session::workspace::ruff_settings No ruff settings file (pyproject.toml/ruff.toml/.ruff.toml) found for /Users/xiaomin/test.py - falling back to default configuration\n  62.696950s ERROR ruff_server::server::api An error occurred with result ID 2: failed to deserialize diagnostic data: missing field `range`\n"
[DEBUG][2024-05-02 23:55:00] .../vim/lsp/rpc.lua:387	"rpc.receive"	{
  jsonrpc = "2.0",
  method = "window/showMessage",
  params = {
    message = "Ruff encountered a problem. Check the logs for more details.",
    type = 1
  }
}
[DEBUG][2024-05-02 23:55:00] .../vim/lsp/rpc.lua:387	"rpc.receive"	{
  error = {
    code = -32603,
    message = "failed to deserialize diagnostic data: missing field `range`"
  },
  id = 2,
  jsonrpc = "2.0"
}
[TRACE][2024-05-02 23:55:00] .../lua/vim/lsp.lua:1052	"notification"	"window/showMessage"	{
  message = "Ruff encountered a problem. Check the logs for more details.",
  type = 1
}
[TRACE][2024-05-02 23:55:00] ...lsp/handlers.lua:618	"default_handler"	"window/showMessage"	{
  ctx = '{\n  client_id = 1,\n  method = "window/showMessage"\n}',
  result = {
    message = "Ruff encountered a problem. Check the logs for more details.",
    type = 1
  }
}
[ERROR][2024-05-02 23:55:00] .../vim/lsp/rpc.lua:734	"rpc"	"/Users/xiaomin/.local/share/nvim/mason/bin/ruff-lsp"	"stderr"	"Exception occurred for message \"2\": KeyError: 'location'\nTraceback (most recent call last):\n  File \"/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/pygls/protocol/json_rpc.py\", line 194, in _execute_request_callback\n    self._send_response(msg_id, result=future.result())\n                                       ^^^^^^^^^^^^^^^\n  File \"/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py\", line 1000, in code_action\n    edit=_create_workspace_edit(\n         ^^^^^^^^^^^^^^^^^^^^^^^\n  File \"/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py\", line 1490, in _create_workspace_edit\n    line=edit[\"location\"][\"row\"] - 1,\n         ~~~~^^^^^^^^^^^^\nKeyError: 'location'\n"
[DEBUG][2024-05-02 23:55:00] .../vim/lsp/rpc.lua:387	"rpc.receive"	{
  error = {
    code = -32603,
    data = {
      traceback = { '  File "/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/pygls/protocol/json_rpc.py", line 194, in _execute_request_callback\n    self._send_response(msg_id, result=future.result())\n                                       ^^^^^^^^^^^^^^^\n', '  File "/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py", line 1000, in code_action\n    edit=_create_workspace_edit(\n         ^^^^^^^^^^^^^^^^^^^^^^^\n', '  File "/Users/xiaomin/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py", line 1490, in _create_workspace_edit\n    line=edit["location"]["row"] - 1,\n         ~~~~^^^^^^^^^^^^\n' }
    },
    message = "KeyError: 'location'"
  },
  id = 2,
  jsonrpc = "2.0"
}
```

I'm using `ruff 0.4.2` and `ruff-lsp 0.0.53` installed with Mason. It works fine if you disable one or the other so probably not a big deal 😅

---

_Comment by @dhruvmanila on 2024-05-03 04:51_

Interesting find! It's because both `ruff_lsp` and `ruff server` sends the diagnostics with the same name "Ruff". So, when you invoke code actions, Neovim sends all (from both `ruff server` and `ruff_lsp`) the diagnostics on the cursor line to all the servers (both `ruff server` and `ruff_lsp`). But, the structure of diagnostic expected by `ruff server` and `ruff_lsp` is different which is why you're getting a key error.

It's not recommended to use `ruff-lsp` and the new server at the same time. I think we should make this clear in the documentation if it's not mentioned.

---

_Comment by @dhruvmanila on 2024-05-03 04:57_

Ok, it's at least mentioned for the Neovim setup guide: https://github.com/astral-sh/ruff/blob/main/crates/ruff_server/docs/setup/NEOVIM.md

And I don't think it's possible to even run both `ruff_lsp` and `ruff server` in VS Code at the same time because they both are available exclusively in the stable and pre-release version of the extension. Even in pre-release one can only enable either one of the server.

I'll close this issue as it's not recommended to run both the server at the same time.

cc @snowsignal

---

_Closed by @dhruvmanila on 2024-05-03 04:57_

---

_Label `server` added by @dhruvmanila on 2024-05-03 04:57_

---

_Comment by @xl4624 on 2024-05-03 05:10_

Makes sense. Not even sure what compelled me to install `ruff_lsp` to be honest, I probably installed it by accident one day and just ran with it. 

Thanks for the response!

---
