```yaml
number: 12778
title: Unable to set lineLength from nvim lua config
type: issue
state: closed
author: SafetyMary
labels:
  - question
  - server
assignees: []
created_at: 2024-08-09T10:01:35Z
updated_at: 2025-01-07T05:17:22Z
url: https://github.com/astral-sh/ruff/issues/12778
synced_at: 2026-01-12T15:54:52Z
```

# Unable to set lineLength from nvim lua config

---

_@SafetyMary_

Not sure if I did this correctly but i couldnt set --line-length arg in my neovim config (setting it to 1 just for testing). I am using kickstart vim.

Inside my neovim/nvim-lspconfig
![image](https://github.com/user-attachments/assets/d2a56d19-0d1b-48f2-ac07-e93e46de722b)

The formatting runs fine on ":Format" command but ignores my line length setting. The only way I could get it work is to run "!ruff format --line-length 1" manually.

Version:
ruff 0.5.7
NVIM v0.10.1

---

_Comment by @dhruvmanila on 2024-08-09 11:03_

What does the `:Format` command do? Is it something that you've defined in your own config or is it coming from an external plugin? If the latter, which plugin is it coming from?

The language server settings will only be accounted for when running the formatter via the server aka `vim.lsp.buf.format()`.

---

_Label `needs-info` added by @dhruvmanila on 2024-08-09 11:03_

---

_Comment by @SafetyMary on 2024-08-09 15:37_

I have set a nvim user command triggering the formater. Mostly copied from conform.nvim recipy. 

```lua
-- Add format command
vim.api.nvim_create_user_command('Format', function(args)
  local range = nil
  if args.count ~= -1 then
    local end_line = vim.api.nvim_buf_get_lines(0, args.line2 - 1, args.line2, true)[1]
    range = {
      start = { args.line1, 0 },
      ['end'] = { args.line2, end_line:len() },
    }
  end
  require('conform').format { async = true, lsp_format = 'fallback', range = range }
end, { range = true })
```

Below is my conform formatter set up, mostly copied from kickstart vim

```lua
-- kickstart config
return {
  { -- Autoformat
    'stevearc/conform.nvim',
    event = { 'BufWritePre' },
    cmd = { 'ConformInfo' },
    keys = {
      {
        '<leader>f',
        function()
          require('conform').format { async = true, lsp_fallback = true }
        end,
        mode = '',
        desc = '[F]ormat buffer',
      },
    },
    opts = {
      notify_on_error = false,
      format_on_save = function(bufnr)
        -- Define default behaviour here (default disable autoformat)
        if (vim.g.disable_autoformat == nil) or (vim.b[bufnr].disable_autoformat == nil) then
          return
        end
        -- Disable autoformat with a global or buffer-local variable
        if vim.g.disable_autoformat or vim.b[bufnr].disable_autoformat then
          return
        end
        -- Disable "format_on_save lsp_fallback" for languages that don't
        -- have a well standardized coding style. You can add additional
        -- languages here or re-enable it for the disabled ones.
        local disable_filetypes = { c = true, cpp = true }
        return {
          timeout_ms = 500,
          lsp_fallback = not disable_filetypes[vim.bo[bufnr].filetype],
        }
      end,
      formatters_by_ft = {
        lua = { 'stylua' },
        python = { 'ruff_fix', 'ruff_organize_imports', 'ruff_format' },
        sql = { 'sqlfluff' },
        markdown = { 'prettier', 'markdown-toc', 'markdownlint-cli2' },
        -- Conform can also run multiple formatters sequentially
        -- python = { "isort", "black" },
        --
        -- You can use 'stop_after_first' to run the first available formatter from the list
        -- javascript = { "prettierd", "prettier", stop_after_first = true },
      },
    },
  },
}

```

---

_Comment by @dhruvmanila on 2024-08-09 17:35_

There's some context here: https://github.com/astral-sh/ruff/issues/12514#issuecomment-2252147682 (and following replies) but the gist is that `conform.nvim` (specifically the `ruff_organize_imports` and `ruff_format` values) uses the `ruff` binary to perform the actions. The `lineLength` setting is specific to the server, so it will only work if you run the formatting via the language server (`vim.lsp.buf.format()`).

---

_Comment by @dhruvmanila on 2024-08-09 17:37_

Note that `conform.nvim` is a formatter plugin which can use various ways of running a specific formatter. It could use the executable or the language server although you can configure it to either always use the language server or prefer it first.

---

_Closed by @dhruvmanila on 2024-08-09 17:37_

---

_Label `needs-info` removed by @dhruvmanila on 2024-08-09 17:37_

---

_Label `question` added by @dhruvmanila on 2024-08-09 17:37_

---

_Comment by @dhruvmanila on 2024-08-10 05:24_

You can configure `conform.nvim` to run the formatting via the language server:
```lua
require('conform').setup {
  formatters_by_ft = {
    python = { 'ruff_fix', 'ruff_organize_imports', lsp_format = 'first' },
  }
}
```
This way the server settings will be considered during formatting via the Ruff language server. You can verify whether `conform.nvim` is going to use Ruff LSP via `:ConformInfo` which should show:

```
Formatters for this buffer:
LSP: ruff
ruff_fix ready (python) /Users/dhruv/.local/bin/ruff
ruff_organize_imports ready (python) /Users/dhruv/.local/bin/ruff
```

> [!NOTE]
>
> The `ruff_fix` and `ruff_organize_imports` will still use the `ruff` binary and the server settings will not be considered for those runs

---

_Comment by @SafetyMary on 2024-08-12 02:15_

Thanks for the help @dhruvmanila 
I have tried using your suggested snipet but it seems the line length setting only works when calling 
```lua
require('conform').setup {
  formatters_by_ft = {
    python = { lsp_format = 'first' },
  }
}
```
*without ruff_fix and ruff_organize_imports

I tried to change the sequence but couldnt solve the issue. For now i couldnt find a way getting both "line length" and "ruff_fix + ruff_ogranize_import" to work together.

---

_Comment by @SafetyMary on 2024-08-12 02:48_

~~I have got organize imports working by setting "organizeImports = true" and removing "ruff_organize_import" from the list.~~

```lua
-- in my lspconfig.lua
ruff = {
  init_options = {
    settings = {
      lineLength = 1,
      organizeImports = true,
    }
  }
},
```

```lua
-- in my conform.lua
require('conform').setup {
  formatters_by_ft = {
    python = { lsp_format = 'first' },
  }
}
```

I still quite new to neovim/lua/ruff and not quite sure what ruff_fix does. Will check on the "fixAll" option in ruff config.

---

_Comment by @dhruvmanila on 2024-08-12 03:53_

> I have got organize imports working by setting "organizeImports = true" and removing "ruff_organize_import" from the list.

Huh, that's interesting. What do you mean by "have got organize imports working"? I'm asking because the default value for that setting _is_ `true`.

As noted above, the `ruff_*` values in `conform.nvim` will use the `ruff` executable to perform the actions. For example, you can see that `ruff_format` just calls the `ruff format` command: https://github.com/stevearc/conform.nvim/blob/667102f26106709cddd2dff1f699610df5b94d7f/lua/conform/formatters/ruff_format.lua#L7-L14. This means that the [**server settings**](https://docs.astral.sh/ruff/editors/settings/) won't be considered there. Server settings are only considered when running the Ruff language server.

`ruff_fix` basically calls the `ruff check --fix` command as seen in https://github.com/stevearc/conform.nvim/blob/667102f26106709cddd2dff1f699610df5b94d7f/lua/conform/formatters/ruff_fix.lua#L7-L17 which is auto-fixing the rules that are fixable.

---

_Comment by @SafetyMary on 2024-08-14 09:20_

Sorry my bad. Upon futher inspection, it seems ruff organize imports is not working despite explisitly setting organizeImports = true, lineLength setting works fine though. I have also tried added select = {'I'} but it still doesnt work

```lua
        ruff = {
          init_options = {
            settings = {
              lineLength = 120,
              organizeImports = true,
              fixAll = true,
              lint = {
                select = { 'I' },
              },
            },
          },
        },
```

---

_Comment by @SafetyMary on 2024-08-15 02:59_

I have fixed the issue by falling back to using ruff CLI args as i could not get it working via language server.

```lua
-- Add this line to conform.lua
 require('conform').formatters.ruff_format = { append_args = { '--line-length', '120' } }

-- python formaters in conform.lua
python = { 'ruff_fix', 'ruff_organize_imports', 'ruff_format' },

```

---

_Comment by @dhruvmanila on 2024-08-16 03:56_

As mentioned before, all these formatter configuration (`ruff_fix`, `ruff_organize_imports`, `ruff_format`) uses the Ruff executable i.e., they basically call the `ruff` executable to do the job. To use the server settings, you'd need to perform all three actions by asking the language server instead.

---

_Label `server` added by @dhruvmanila on 2025-01-07 05:17_

---
