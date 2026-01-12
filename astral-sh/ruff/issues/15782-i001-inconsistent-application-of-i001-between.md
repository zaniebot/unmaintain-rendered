```yaml
number: 15782
title: "I001: Inconsistent application of I001 between neovim and vscode"
type: issue
state: closed
author: leonlonsdale
labels:
  - question
  - server
assignees: []
created_at: 2025-01-28T11:00:03Z
updated_at: 2025-02-06T08:45:42Z
url: https://github.com/astral-sh/ruff/issues/15782
synced_at: 2026-01-12T15:54:54Z
```

# I001: Inconsistent application of I001 between neovim and vscode

---

_@leonlonsdale_

### Description

Hey guys, 

I'm new to coding and even newer to python, so this is likely a skill issue.

I switch between vscode and neovim. Both are set up to use `~/.config/ruff/ruff.toml`:

VSCode:

```json
"ruff.configuration": "~/.config/ruff/ruff.toml",
```

Neovim:

```lua
init_options = {
    settings = {
        configuration = "~/.config/ruff/ruff.toml",
    },
},
```

I have stripped the settings in both editors right back so that they only point to the config file, and make no other changes. There is no local project config file. Caches have been cleared.

The contents of the ruff configuration are:

```toml
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".ipynb_checkpoints",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pyenv",
    ".pytest_cache",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    ".vscode",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
]


line-length = 100
indent-width = 4

[lint]
select = [
    "E", "F", "W", "B", "I", "RUF", "N", "LOG", "ERA", "D", "UP",
    "ANN", "ASYNC", "S", "RET", "TCH", "ARG", "PTH"
]
ignore = [
    "D203", "D213"
]

fixable  = ["ALL"]
extend-fixable = ["E", "F", "I"]

[lint.isort]
case-sensitive = true
order-by-type = true
combine-as-imports = true
length-sort = true
lines-between-types = 1
no-sections = false


[format]
quote-style = "double"
indent-style = "space"
line-ending = "auto"
docstring-code-format = true
docstring-code-line-length = "dynamic"
```

Both are using ruff version 0.9.3 with neovim managing it via Mason and homebrew managing globally.

Using ruff to organise imports in each of them results in a different order, with neovim sorting as follows:

```
import os

import fetcher as f
import stock_calculations as sc

from data import data
from dotenv import load_dotenv
from emailer import send_email
```

and vscode then showing the I001 warning. Applying organize imports in vscode then using ruff gives:

```
import os

from dotenv import load_dotenv

import fetcher as f
import stock_calculations as sc

from data import data
from emailer import send_email
```

which of course then flags the I001 warning in neovim.



Note: fetcher, stock_calculations, data, and emailer are custom modules and they are all saved in root:

```
 .
├── data.py
├── dates.py
├── emailer.py
├── fetcher.py
├── main.py
├── stock_calculations.py
```

Running `ruff check -v main.py` in the console gives:

```
[2025-01-28][10:51:49][ruff::diagnostics][DEBUG] Checking: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'os' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fetcher' as Known(FirstParty) (SourceMatch("/.../100-days-of-python/intermediate-plus/day-36-stock-trading/"))
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'stock_calculations' as Known(FirstParty) (SourceMatch("/.../100-days-of-python/intermediate-plus/day-36-stock-trading/"))
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'emailer' as Known(FirstParty) (SourceMatch("/.../100-days-of-python/intermediate-plus/day-36-stock-trading/"))
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'dotenv' as Known(ThirdParty) (NoMatch)
[2025-01-28][10:51:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'data' as Known(FirstParty) (SourceMatch("/.../100-days-of-python/intermediate-plus/day-36-stock-trading/"))
[2025-01-28][10:51:50][ruff::commands::check][DEBUG] Checked 1 files in: 22.259541ms

```

Hoping someone can point me in the right direction with this, I've been trying to get them to behave the same now for a good few hours.

Thanks

https://github.com/user-attachments/assets/c5772f5a-8d55-46cc-a5e9-4a1b6593488b

---

_Renamed from "Inconsistent application of I001 between neovim and vscode" to "I001: Inconsistent application of I001 between neovim and vscode" by @leonlonsdale on 2025-01-28 11:23_

---

_Label `isort` added by @AlexWaygood on 2025-01-28 11:51_

---

_Comment by @dhruvmanila on 2025-01-29 10:35_

Thanks for the report.

I think the order in Neovim is correct because `dotenv` is a third-party import while others are first-party imports.

Trying to reproduce in VS Code seems to be giving the same outcome as in Neovim. Can you provide the VS Code debug logs (refer to the [troubleshooting guide](https://github.com/astral-sh/ruff-vscode#troubleshooting).

Directory structure:
```
❯ tree .
.
├── data.py
├── dates.py
├── emailer.py
├── fetcher.py
├── main.py
└── stock_calculations.py
```

VS Code settings:

```jsonc
{
  "ruff.nativeServer": "on",
  "ruff.logLevel": "debug",
  "ruff.configuration": "/tmp/ruff-config/ruff.toml"
}
```

---

_Label `needs-info` added by @dhruvmanila on 2025-01-29 10:35_

---

_Comment by @leonlonsdale on 2025-01-29 11:29_

Hey, sure, using these settings:

```
{
  "ruff.lineLength": 100,
  "ruff.lint.preview": true,
  "ruff.trace.server": "verbose",
  "ruff.nativeServer": "on",
  "ruff.logLevel": "debug",
  "ruff.configuration": "~/.config/ruff/ruff.toml"
}
```

**The client output is:**

```
2025-01-29 11:22:09.227 [info] Name: Ruff
2025-01-29 11:22:09.227 [info] Module: ruff
2025-01-29 11:22:09.228 [info] Python extension loading
2025-01-29 11:22:09.228 [info] Waiting for interpreter from python extension.
2025-01-29 11:22:09.228 [info] Python extension loaded
2025-01-29 11:22:09.228 [info] Using interpreter: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/venv/bin/python
2025-01-29 11:22:09.228 [info] Using environment executable: /opt/homebrew/bin/ruff
2025-01-29 11:22:09.230 [info] Found Ruff 0.9.3 at /opt/homebrew/bin/ruff
2025-01-29 11:22:09.230 [info] Server run command: /opt/homebrew/bin/ruff server
2025-01-29 11:22:09.231 [info] Server: Start requested.
```

**Server output:**

```
   0.022056334s DEBUG main ruff_server::session::index::ruff_settings: Indexing settings for workspace: /.../100-days-of-python/intermediate-plus/day-36-stock-trading
   0.051389459s DEBUG main ruff_server::session::index::ruff_settings: Combining settings from editor-specified configuration file at: /.../.config/ruff/ruff.toml
   0.074706417s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/.ruff_cache
   0.077459334s  INFO main ruff_server::session::index: Registering workspace: /.../100-days-of-python/intermediate-plus/day-36-stock-trading
   0.085180500s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
   5.452550750s DEBUG ruff:worker:7 ruff_server::resolve: Included path via `include`: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py
  22.891624417s DEBUG ruff:worker:4 ruff_server::resolve: Included path via `include`: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py
  27.867928417s DEBUG ruff:worker:1 ruff_server::resolve: Included path via `include`: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py
  27.876309917s DEBUG ruff:worker:0 ruff_server::resolve: Included path via `include`: /.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py
```

**Unsure if this is in anyway useful, but here is the LS Trace too:**

```
[Trace - 11:53:31] Sending request 'textDocument/codeAction - (66)'.
Params: {
    "textDocument": {
        "uri": "file:///.../coding/100-days-of-python/intermediate-plus/day-36-stock-trading/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 0
        }
    },
    "context": {
        "diagnostics": [],
        "only": [
            "source.organizeImports"
        ],
        "triggerKind": 1
    }
}


[Trace - 11:53:31] Received response 'textDocument/codeAction - (66)' in 3ms.
Result: [
    {
        "data": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
        "kind": "source.organizeImports.ruff",
        "title": "Ruff: Organize imports"
    }
]


[Trace - 11:53:31] Sending request 'codeAction/resolve - (67)'.
Params: {
    "title": "Ruff: Organize imports",
    "data": "file:///.../coding/100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
    "kind": "source.organizeImports.ruff"
}


[Trace - 11:53:31] Received response 'codeAction/resolve - (67)' in 5ms.
Result: {
    "data": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
    "edit": {
        "documentChanges": [
            {
                "edits": [
                    {
                        "newText": "from dotenv import load_dotenv\n\nimport fetcher as f\nimport stock_calculations as sc\nfrom data import data\n",
                        "range": {
                            "end": {
                                "character": 0,
                                "line": 8
                            },
                            "start": {
                                "character": 0,
                                "line": 4
                            }
                        }
                    }
                ],
                "textDocument": {
                    "uri": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
                    "version": null
                }
            }
        ]
    },
    "kind": "source.organizeImports.ruff",
    "title": "Ruff: Organize imports"
}


[Trace - 11:53:31] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
        "version": 5
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 6,
                    "character": 21
                },
                "end": {
                    "line": 7,
                    "character": 30
                }
            },
            "rangeLength": 31,
            "text": ""
        },
        {
            "range": {
                "start": {
                    "line": 4,
                    "character": 0
                },
                "end": {
                    "line": 4,
                    "character": 0
                }
            },
            "rangeLength": 0,
            "text": "from dotenv import load_dotenv\n\n"
        }
    ]
}


[Trace - 11:53:31] Sending request 'textDocument/diagnostic - (68)'.
Params: {
    "identifier": "Ruff",
    "textDocument": {
        "uri": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py"
    }
}


[Trace - 11:53:31] Received response 'textDocument/diagnostic - (68)' in 3ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 11:53:32] Sending request 'textDocument/codeAction - (69)'.
Params: {
    "textDocument": {
        "uri": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 0
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


[Trace - 11:53:32] Received response 'textDocument/codeAction - (69)' in 6ms.
Result: [
    {
        "data": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
        "kind": "source.fixAll.ruff",
        "title": "Ruff: Fix all auto-fixable problems"
    },
    {
        "data": "file:///.../100-days-of-python/intermediate-plus/day-36-stock-trading/main.py",
        "kind": "source.organizeImports.ruff",
        "title": "Ruff: Organize imports"
    }
]
```

**The result:**

```
import os

from dotenv import load_dotenv

import fetcher as f
import stock_calculations as sc
from data import data
from emailer import send_email
```

Though, with dotenv being third party, I would expect it to be organised separately to the first party imports, as vscode is doing right? it seems to be in amongst the first party imports in neovim, and just organised along with the other `from` imports. Or have I got that wrong?

---

_Comment by @MichaReiser on 2025-02-03 08:05_

I agree that the VS code behavior is the expected behavior with `dotenv` in its own group which, if I understand your description correctly, also matches the CLI behavior. So the question is why neovim disagrees :) 

I somewhat suspect that Neovim categorizes the imports differently, maybe because the root directory is different. Can you try enable logging in neovim as described at the end [of the neovim setup instructions](https://docs.astral.sh/ruff/editors/setup/#neovim) and share the logs with us?



---

_Comment by @dhruvmanila on 2025-02-03 10:18_

Ah, sorry, I diagnosed it incorrectly. You can get the root directory using for the buffer where Ruff is currently running in:
```lua
vim.lsp.get_clients({name = 'ruff', buffer = 0})[1].root_dir
```

---

_Comment by @leonlonsdale on 2025-02-04 14:40_

Let me give these a try and come back to you

---

_Comment by @leonlonsdale on 2025-02-04 14:47_

```
[START][2025-02-04 14:42:56] LSP logging initiated
[ERROR][2025-02-04 14:42:56] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/.../.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.000683041s  INFO main ruff_server::server: No workspace settings found for file:///Users/.../100days, using default settings\n"
[ERROR][2025-02-04 14:42:56] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/.../.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.044479125s  INFO main ruff_server::session::index: Registering workspace: /Users/.../100days\n"
[ERROR][2025-02-04 14:42:56] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/.../.local/share/nvim/mason/bin/ruff"	"stderr"	"   0.048438625s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered\n"
```

You may be onto something

> Ah, sorry, I diagnosed it incorrectly. You can get the root directory using for the buffer where Ruff is currently running in:
> 
> vim.lsp.get_clients({name = 'ruff', buffer = 0})[1].root_dir

This gave an error but in :LspInfo, I have:

```
   24 - Client: ruff (id: 2, bufnr: [6])
   25   root directory:    ~/Library/Mobile Documents/com~apple~CloudDocs/Personal/study/coding/100days/
   26   filetypes:         python
   27   cmd:               ~/.local/share/nvim/mason/bin/ruff server
   28   version:           ruff 0.9.4
   29   executable:        true
   30   autostart:         true
```

I also ran:

```
:lua vim.fn.writefile({vim.inspect(vim.lsp.get_active_clients())}, vim.fn.expand("~") .. "/lsp_clients.txt")
```

Upon inspecting the file, I see:

```
config = {
    _on_attach = <function 6>,
    autostart = true,
    capabilities = <table 1>,
    cmd = { "/Users/leonlonsdale/.local/share/nvim/mason/bin/ruff", "server" },
    cmd_cwd = "/Users/leonlonsdale/Library/Mobile Documents/com~apple~CloudDocs/Personal/study/coding/100days",
    filetypes = { "python" },
    handlers = <table 35>,
    init_options = <36>vim.empty_dict(),   <-- 🚨
    log_level = 2,
    message_level = 2,
    name = "ruff",
    on_attach = <function 1>,
    on_exit = <function 2>,
    on_init = <function 4>,
    root_dir = "/Users/leonlonsdale/Library/Mobile Documents/com~apple~CloudDocs/Personal/study/coding/100days",
    settings = <37>{},  <-- 🚨
    single_file_support = true,
    workspace_folders = <38>{ { 
        name = "/Users/leonlonsdale/Library/Mobile Documents/com~apple~CloudDocs/Personal/study/coding/100days",
        uri = "file:///Users/leonlonsdale/Library/Mobile%20Documents/com~apple~CloudDocs/Personal/study/coding/100days"
    } }
}
```

So it looks as though init_options and settings are empty tables.


---

_Comment by @leonlonsdale on 2025-02-04 15:52_

Guys, I have fixed it, it was an issue with the way I was loading lsp configs. I created my own loader and it turns out it had a bug that was being missed because the nvim_lsp default configs were taking over giving it the appearance it had worked. 

Sorry for wasting your time, and thank you for your help.


---

_Comment by @MichaReiser on 2025-02-04 17:51_

No worries. Thanks for posting an update. Enjoy Ruff

---

_Closed by @MichaReiser on 2025-02-04 17:51_

---

_Label `needs-info` removed by @dhruvmanila on 2025-02-06 08:45_

---

_Label `question` added by @dhruvmanila on 2025-02-06 08:45_

---

_Label `isort` removed by @dhruvmanila on 2025-02-06 08:45_

---

_Label `server` added by @dhruvmanila on 2025-02-06 08:45_

---
