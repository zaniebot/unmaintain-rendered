```yaml
number: 12514
title: Neovim - issue with configuration/configurationPreference
type: issue
state: closed
author: steakhutzeee
labels:
  - server
assignees: []
created_at: 2024-07-25T16:53:37Z
updated_at: 2024-07-27T02:38:53Z
url: https://github.com/astral-sh/ruff/issues/12514
synced_at: 2026-01-10T11:09:54Z
```

# Neovim - issue with configuration/configurationPreference

---

_Issue opened by @steakhutzeee on 2024-07-25 16:53_

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

Hello, I'm configuring ruff server 0.5.5 for Neovim and having some issues:

1. First thing I noted is that this setting https://docs.astral.sh/ruff/editors/settings/#configurationpreference works also on Neovim, even if the docs specify that it works on VS Code.
2. About the setting mentioned above, I think that `"filesystemFirst"` should be the default. Apart from my personal ruff settings, if working on a someone else's project, I think I should use the setting the user of the project intended for it, and not the ones I specified for my client.
3. With the above setting enabled/disabled, I'm not able to let the setting https://docs.astral.sh/ruff/editors/settings/#configuration to work. I intend to use it to set some values not actually exposed by the ruff server.

This is my Neovim configuration in `/home/user/.config/nvim/lua/user/lspsettings/ruff.lua`:

```
return {
  init_options = {
    settings = {
      configuration = "/home/user/.config/nvim/lua/user/lspsettings/ruff.toml",
      lineLength = 88,
      configurationPreference = "filesystemFirst",
      lint = {
        select = {"ALL"},
        ignore = {
          -- Prevent duplicates with basedpyright
          "ANN",
          "B018",
          "F821",
          "F401",
          "PLC0414",
          "RUF013",
          "RUF016"
        }
      }
    }
  }
}
```
Tried also disabling `configurationPreference`.

And tried with the following `/home/user/.config/nvim/lua/user/lspsettings/ruff.toml`:

`indent-width = 2`

But it does not work. I tried with a python file related to an existing pyproject.toml file and also with a test file without any pyproject.toml file. Indent is still set to 4.

Any idea? Thank you!



---

_Comment by @steakhutzeee on 2024-07-25 17:11_

From further tests it looks that:

1. If `configurationPreference = "filesystemFirst"` the `configuration` setting does not work with files related to an existing pyproject.toml. I'm fine with this, I think it works ok this way. So it's just extending the client config, but that's ignored if there is some other config file in the workspace.
2. Tried setting another option in my `/home/user/.config/nvim/lua/user/lspsettings/ruff.toml` and it works, so the problem is with `indent-width`?

```
indent-width = 2
line-length = 5 #This works.
```

Tested with another random option and it does not work:

```
indent-width = 2
line-length = 5 #Only this works.
[format]
quote-style = "single"
```


---

_Comment by @dhruvmanila on 2024-07-26 06:33_

> 1. First thing I noted is that this setting [docs.astral.sh/ruff/editors/settings#configurationpreference](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) works also on Neovim, even if the docs specify that it works on VS Code.

Yes, those are server settings which works on all editors. Can you tell me why you thought that it only works on VS Code? Could we improve the documentation to make it clear? The example usage provides one for Neovim as well.

> 2. About the setting mentioned above, I think that `"filesystemFirst"` should be the default. Apart from my personal ruff settings, if working on a someone else's project, I think I should use the setting the user of the project intended for it, and not the ones I specified for my client.

There's some discussion about this in https://github.com/astral-sh/ruff-vscode/issues/425 and https://github.com/astral-sh/ruff-vscode/issues/3.

> 3. With the above setting enabled/disabled, I'm not able to let the setting [docs.astral.sh/ruff/editors/settings#configuration](https://docs.astral.sh/ruff/editors/settings/#configuration) to work. I intend to use it to set some values not actually exposed by the ruff server.

Ok, I think there's some problem in here. I'm scratching my head to figure it out.

---

_Label `server` added by @dhruvmanila on 2024-07-26 06:33_

---

_Comment by @dhruvmanila on 2024-07-26 06:41_

> Ok, I think there's some problem in here. I'm scratching my head to figure it out.

Ugh, nevermind, it was a silly mistake from my end.

Ok, so I tried your setup and it's working fine for me. Here's what I've tried:
1. Create a `/tmp/ruff.toml` with `indent-width = 2`
2. Using the following Neovim config
```lua
require('lspconfig').ruff.setup({
  init_options = {
    settings = {
      configuration = '/tmp/ruff.toml',
      configurationPreference = 'filesystemFirst',
    },
  },
})
```
3. Open a file in any directory other than `/tmp/` and run the formatter via the language server

https://github.com/user-attachments/assets/78656e16-aa84-44d0-a2e3-4092869aa152

> But it does not work. I tried with a python file related to an existing pyproject.toml file and also with a test file without any pyproject.toml file. Indent is still set to 4.

Can you tell me how you verified this?

Do you have any configuration file that's existing in the project that you've opened in Neovim?

---

_Label `needs-info` added by @dhruvmanila on 2024-07-26 06:58_

---

_Comment by @steakhutzeee on 2024-07-26 07:04_

> > 1. First thing I noted is that this setting [docs.astral.sh/ruff/editors/settings#configurationpreference](https://docs.astral.sh/ruff/editors/settings/#configurationpreference) works also on Neovim, even if the docs specify that it works on VS Code.
> 
> Yes, those are server settings which works on all editors. Can you tell me why you thought that it only works on VS Code? Could we improve the documentation to make it clear? The example usage provides one for Neovim as well.
> 

Hi, thank you for the feedback.

Yes it is just the docs to be a little misleading, stating 

> The strategy to use when resolving settings across VS Code and the filesystem.

---

_Comment by @steakhutzeee on 2024-07-26 07:10_

> > Ok, I think there's some problem in here. I'm scratching my head to figure it out.
> 
> Ugh, nevermind, it was a silly mistake from my end.
> 
> Ok, so I tried your setup and it's working fine for me. Here's what I've tried:
> 
> 1. Create a `/tmp/ruff.toml` with `indent-width = 2`
> 2. Using the following Neovim config
> 
> ```lua
> require('lspconfig').ruff.setup({
>   init_options = {
>     settings = {
>       configuration = '/tmp/ruff.toml',
>       configurationPreference = 'filesystemFirst',
>     },
>   },
> })
> ```
> 
> 3. Open a file in any directory other than `/tmp/` and run the formatter via the language server
> 
>  Screen.Recording.2024-07-26.at.12.09.15.mov 
> > But it does not work. I tried with a python file related to an existing pyproject.toml file and also with a test file without any pyproject.toml file. Indent is still set to 4.
> 
> Can you tell me how you verified this?
> 
> Do you have any configuration file that's existing in the project that you've opened in Neovim?

Ok, I only tried using `:=vim.lsp.buf.format()` as your example and it works as inteded.

But it does not work when using https://github.com/stevearc/conform.nvim that I use with the following config:

```
local M = {
  'stevearc/conform.nvim'
}

function M.config()
  require("conform").setup({
    formatters_by_ft = {
      python = {'ruff_organize_imports', 'ruff_format'},
    },
    format_after_save = {
      lsp_fallback = true,
    },
    notify_on_error = true,
  })
end

return M
```

---

_Comment by @dhruvmanila on 2024-07-26 07:32_

Yeah, that's because the `ruff_format` as specified in your `conform.nvim` setup uses the `ruff` executable. IOW, it doesn't use the language server to format the buffer and the settings that you specify in `require('lspconfig').ruff.setup` is only relevant in that context.

---

_Comment by @dhruvmanila on 2024-07-26 07:34_

Closing this issue as resolved.

---

_Closed by @dhruvmanila on 2024-07-26 07:34_

---

_Label `needs-info` removed by @dhruvmanila on 2024-07-26 07:34_

---

_Comment by @steakhutzeee on 2024-07-26 07:38_

> Yeah, that's because the `ruff_format` as specified in your `conform.nvim` setup uses the `ruff` executable. IOW, it doesn't use the language server to format the buffer and the settings that you specify in `require('lspconfig').ruff.setup` is only relevant in that context.

Thank you, this means that actually using Conform the Ruff language server formatter will not work at all? Do you know of any way this can be fixed or should be requested upstream to Conform?

Also, for the import sorting, it looks like Conform still works, so for that I can continue to use it?

---

_Comment by @dhruvmanila on 2024-07-26 12:04_

No, you can still use `conform.nvim` but you need to explicitly tell it to use the language server to format the buffer instead of the CLI. So, I think you'd need to remove all the formatters for the Python language from the list (`ruff_organize_imports`, `ruff_format`) so that it falls back to using the language server. But, then you loose the functionality to organize imports. My recommendation would be to use either the language server or the CLI for everything. It's fine to use either of them at the same time but then the settings which are specific to the one cannot be used with the other, for example, using the `ruff` CLI won't see the server settings set in your Neovim config.

---

_Comment by @steakhutzeee on 2024-07-26 12:21_

> No, you can still use `conform.nvim` but you need to explicitly tell it to use the language server to format the buffer instead of the CLI. So, I think you'd need to remove all the formatters for the Python language from the list (`ruff_organize_imports`, `ruff_format`) so that it falls back to using the language server. But, then you loose the functionality to organize imports. My recommendation would be to use either the language server or the CLI for everything. It's fine to use either of them at the same time but then the settings which are specific to the one cannot be used with the other, for example, using the `ruff` CLI won't see the server settings set in your Neovim config.

Thank you, indeed I can format disabling the formatters. Any hint on how I could also use the language server for the import sorting?

Maybe I could also implement the following (that I had commented out because I was using Conform for everything):

```
vim.api.nvim_create_autocmd({ "BufWritePost" }, {
  pattern = { "*.py" },
  callback = function()
    vim.lsp.buf.code_action {
      context = {
        only = { 'source.organizeImports.ruff' },
      },
      apply = true,
    }
  end,
})
```

---

_Comment by @dhruvmanila on 2024-07-26 12:47_

Yeah, that should work. The benefit of plugins like `conform.nvim` is that it would run multiple formatters sequentially which is what you want.

---

_Comment by @steakhutzeee on 2024-07-26 15:30_

> Yeah, that should work. The benefit of plugins like `conform.nvim` is that it would run multiple formatters sequentially which is what you want.

I see, maybe there is a way to run the autocommand from Conform, in order with the other formatter.

Anyway, which should run first? The formatter or the import sorter? 


---

_Comment by @dhruvmanila on 2024-07-27 02:38_

> Anyway, which should run first? The formatter or the import sorter?

I don't think the order matters in this case.

---
