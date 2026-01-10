```yaml
number: 12795
title: How to disable Ruff linter in AstroNvim LSP?
type: issue
state: closed
author: memeLordo
labels:
  - question
assignees: []
created_at: 2024-08-10T15:16:42Z
updated_at: 2024-08-13T16:01:50Z
url: https://github.com/astral-sh/ruff/issues/12795
synced_at: 2026-01-10T11:09:54Z
```

# How to disable Ruff linter in AstroNvim LSP?

---

_Issue opened by @memeLordo on 2024-08-10 15:16_

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
Is there anyway to disable `ruff ` linting completely and leave it only for formattiong? I'm using a NeoVim configuration with Mason. What I have is a double display for linting in `basedpyright` and `ruff`:
![Error](https://github.com/user-attachments/assets/89182f2b-04a3-4f3f-8f0a-803e4309eeb2)

---

_Comment by @memeLordo on 2024-08-10 15:20_

My previous attemts to set up configuration using help from #12514 and #12778 had no effect.

---

_Comment by @MichaReiser on 2024-08-10 15:50_

Have you tried setting [lint.enable](https://docs.astral.sh/ruff/editors/settings/#lint_enable)

If you need more help, please share your neovim configuration.

---

_Comment by @memeLordo on 2024-08-11 02:16_

Sent request. My config is used by AstroNvim. I tried to put the code at different places, but still nothing seems to work.

---

_Comment by @dhruvmanila on 2024-08-11 05:49_

Can you share your Neovim config? I'm specifically interested in how you've setup basedpyright and Ruff. Which plugins are you using to set them up? Is it just `nvim-lspconfig` or do you have any other plugins as well?

---

_Label `question` added by @dhruvmanila on 2024-08-11 05:50_

---

_Comment by @memeLordo on 2024-08-11 13:32_

Basically,  `basedpyright` and `ruff` installed via `Mason`, and it configures using AstroNmiv nested structure. Unfortunately, I haven't been able to find the same configuration for `ruff`, because it implies that user would be using it as a main linter, instead of  `basedpyright`. Here is an example: https://github.com/astral-sh/ruff-lsp/issues/384#issuecomment-1941556771
`ruff` works from the package, and I can't manage to configure it inside my nvim-config.

My `basedpyright_config.lua`:
```lua
-- if true then return {} end -- INFO: REMOVE THIS LINE TO ACTIVATE THIS FILE
return {
  "AstroNvim/astrolsp",
  opts = {

    servers = {
      "basedpyright",
    },

    ---@diagnostic disable: missing-fields
    config = {
      basedpyright = {
        before_init = function(_, c)
          if not c.settings then c.settings = {} end
          if not c.settings.python then c.settings.python = {} end
          c.settings.python.pythonPath = vim.fn.exepath "python"
        end,
        settings = {
          basedpyright = {
            analysis = {
              -- ignore = { "*" },
              -- autoImportCompletions = true,
              -- autoSearchPaths = true,
              diagnosticMode = "openFilesOnly",
              useLibraryCodeForTypes = true,
              -- reportMissingImports = true,
              typeCheckingMode = "standard",
            },
          },
        },
      },
    },
  },
}
```

---

_Comment by @dhruvmanila on 2024-08-12 05:33_

> Unfortunately, I haven't been able to find the same configuration for `ruff`, because it implies that user would be using it as a main linter, instead of `basedpyright`.

I'm not familiar with AstroNvim but are you saying that Ruff is configured to be the default linter in that framework and you're interested in using Ruff only as a formatter? I couldn't find any references to "ruff" in the repository (https://github.com/search?q=repo:AstroNvim/AstroNvim%20ruff&type=code). It's a bit difficult to help without knowing the way you've configured Ruff in your editor 😅 

---

_Comment by @memeLordo on 2024-08-13 13:13_

That's the point, it isn't configured at all, but still works. I will ask in a forum of AstroNvim for this and provide the answer once it's gotten here.

---

_Comment by @memeLordo on 2024-08-13 13:47_

> Have you tried setting [lint.enable](https://docs.astral.sh/ruff/editors/settings/#lint_enable)
> 
> If you need more help, please share your neovim configuration.

Here how you, presumably, can configure the same code in AstroNvim:

`.../plugins/ruff_config.lua`:
```lua
return {
  "AstroNvim/astrolsp",
  opts = {
    config = {
      ruff_lsp = {
        settings = {
          ruff = {
            lint = {
              enable = false,
            },
          },
        },
      },
    },
  },
}
```


---

_Comment by @memeLordo on 2024-08-13 13:48_

But still I get the same double-linting. Will be waiting for the reply from AstroTeam.

---

_Comment by @dhruvmanila on 2024-08-13 13:55_

Can you confirm (via docs?) the name of the Ruff server as defined in the plugin? Is it `ruff_lsp` or `ruff` (native server)? Either way, it should be `init_options` instead of `settings` if these are the server settings.

---

_Comment by @mehalter on 2024-08-13 14:48_

This was resolved by correctly adding the code from the  ruff documentation.

No issue here on the ruff side 😄 

```lua
return {
  "AstroNvim/astrolsp",
  opts = {
    config = {
      ruff = {
        init_options = {
          settings = {
            lint = { enable = false },
          },
        },
      },
    },
  },
}
```

---

_Comment by @memeLordo on 2024-08-13 14:49_

Yeah, it's true. Thank you a lot!

---

_Closed by @memeLordo on 2024-08-13 14:49_

---

_Renamed from "Disable Ruff lint" to "How to disable Ruff linter in AstroNvim LSP?" by @dhruvmanila on 2024-08-13 16:01_

---
