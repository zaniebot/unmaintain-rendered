---
number: 18511
title: No LSP Completions on Neovim
type: issue
state: closed
author: sghng
labels:
  - question
assignees: []
created_at: 2025-06-06T19:07:33Z
updated_at: 2025-06-06T20:25:11Z
url: https://github.com/astral-sh/ruff/issues/18511
synced_at: 2026-01-07T13:12:16-06:00
---

# No LSP Completions on Neovim

---

_Issue opened by @sghng on 2025-06-06 19:07_

### Summary

Followed documentation to setup Ruff for Neovim, diagnostics/linting/formatting all works, but no context-aware completions are given. `LspInfo` shows server is attached correctly.

```
	{ "mason-org/mason.nvim", lazy = true, config = true },
	{
		"mason-org/mason-lspconfig.nvim",
		dependencies = "neovim/nvim-lspconfig",
		event = "VeryLazy",
		config = function()
			require("mason-lspconfig").setup(
				---@type MasonLspconfigSettings
				{
					ensure_installed = {
						"air", -- R LSP and Formatter
						"cssls",
						"html",
						"jsonls",
						"lemminx", -- xml
						"lua_ls",
						"marksman",
						"ruff",
						"tailwindcss",
						"taplo", -- toml LSP
						"ts_ls",
						"unocss",
						"vimls",
						"vue_ls",
					},
				}
			)
}
```

I also tried config the server to run with `ruff server` command instead, still not working.

```
- ruff (id: 1)
  - Version: 0.11.13
  - Root directory: ~/dev/adapta/backend
  - Command: { "ruff", "server" }
  - Settings: {}
  - Attached buffers: 9
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Comment by @sghng on 2025-06-06 19:13_

Switching to `Pyrefly` worked just fine, so it's not caused by Neovim completion settings.

---

_Comment by @ntBre on 2025-06-06 19:23_

Our pre-release [ty language server ](https://github.com/astral-sh/ty/blob/main/docs/README.md#editor-integration) offers some completions (and these are improving rapidly!), but the stable ruff language server [does not](https://docs.astral.sh/ruff/editors/#language-server-protocol):

> The server supports surfacing Ruff diagnostics, providing Code Actions to fix them, and formatting the code using Ruff's built-in formatter. Currently, the server is intended to be used alongside another Python Language Server in order to support features like navigation and autocompletion.

so I believe this is working as intended.

---

_Label `question` added by @ntBre on 2025-06-06 19:24_

---

_Comment by @sghng on 2025-06-06 20:15_

> Our pre-release [ty language server ](https://github.com/astral-sh/ty/blob/main/docs/README.md#editor-integration) offers some completions (and these are improving rapidly!), but the stable ruff language server [does not](https://docs.astral.sh/ruff/editors/#language-server-protocol):
> 
> > The server supports surfacing Ruff diagnostics, providing Code Actions to fix them, and formatting the code using Ruff's built-in formatter. Currently, the server is intended to be used alongside another Python Language Server in order to support features like navigation and autocompletion.
> 
> so I believe this is working as intended.

Thanks for pointing that out! Clearly I overlook that statement in the documentation. It might be a good idea to highlight that in Ruff documentation, and also point users to `ty`. ~~Maybe also add `ty` to Mason registry.~~

---

_Closed by @sghng on 2025-06-06 20:25_

---
