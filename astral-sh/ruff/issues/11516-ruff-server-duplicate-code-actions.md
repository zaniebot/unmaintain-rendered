```yaml
number: 11516
title: "`ruff server`: duplicate code actions"
type: issue
state: closed
author: ravibrock
labels:
  - server
assignees: []
created_at: 2024-05-23T14:57:44Z
updated_at: 2024-05-24T07:20:41Z
url: https://github.com/astral-sh/ruff/issues/11516
synced_at: 2026-01-12T15:54:51Z
```

# `ruff server`: duplicate code actions

---

_@ravibrock_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```md
ruff
    An extremely fast Python linter and code formatter, written in Rust.

    installed version 0.4.5                              
    homepage          https://github.com/astral-sh/ruff/ 
    languages         Python                             
    categories        Linter, Formatter, LSP             
    executables       ruff                               
```

I switched from `ruff-lsp` to `ruff` today in Neovim and noticed that there were duplicate code actions (that as far as I can tell work identically):
<img width="508" alt="Screenshot 2024-05-23 at 09 50 22" src="https://github.com/astral-sh/ruff/assets/66334356/9de1c807-9205-42b8-b262-a6f1146e4f71">
(Note: It appears to only be the `organize imports` and `fix all auto-fixable problems` that are duplicated. The `disable for this line` action only appears once, as it should.)

I tried searching "duplicate code action".

I confirmed that `ruff-lsp` is *not* installed anymore:
<img width="601" alt="Screenshot 2024-05-23 at 09 51 31" src="https://github.com/astral-sh/ruff/assets/66334356/8c7d6119-f014-474f-837e-b8e82d058cf6">

Minimal `ruff` configuration:
```lua
local on_attach = function(client, _)
    if client.name == "ruff" then
        client.server_capabilities.hoverProvider = false
    end
end
require("mason-lspconfig").setup({
    ensure_installed = { "ruff" },
     ["ruff"] = function()
         require("lspconfig").ruff.setup({ on_attach = on_attach })
     end,
})

---

_Label `server` added by @charliermarsh on 2024-05-23 14:58_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-23 14:59_

---

_Closed by @snowsignal on 2024-05-24 07:20_

---
