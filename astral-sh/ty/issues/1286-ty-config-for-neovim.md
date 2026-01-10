```yaml
number: 1286
title: ty config for neovim
type: issue
state: closed
author: Dan7h3x
labels:
  - question
assignees: []
created_at: 2025-09-30T14:56:57Z
updated_at: 2025-10-01T15:30:04Z
url: https://github.com/astral-sh/ty/issues/1286
synced_at: 2026-01-10T02:06:25Z
```

# ty config for neovim

---

_Issue opened by @Dan7h3x on 2025-09-30 14:56_

### Question

How to add support for site-packages in system-wide scope when using ty from neovim?


### Version

_No response_

---

_Label `question` added by @Dan7h3x on 2025-09-30 14:56_

---

_Comment by @sharkdp on 2025-09-30 17:14_

You should be able to use [any of these options to configure ty](https://docs.astral.sh/ty/configuration/) when using ty as an LSP in neovim. You might want to look at [`environment.python`](https://docs.astral.sh/ty/reference/configuration/#python) and [`environment.extra-paths`](https://docs.astral.sh/ty/reference/configuration/#extra-paths).

---

_Comment by @Dan7h3x on 2025-09-30 21:12_

could you give an example of such config for neovim? `vim.lsp` or `lspconfig` doesn't matter


---

_Comment by @MichaReiser on 2025-10-01 06:10_

See here https://docs.astral.sh/ty/editors/#other-editors

---

_Comment by @Dan7h3x on 2025-10-01 07:23_

@MichaReiser Sorry, i already read that, the issue is those environment options not works with `ty server` which `vim.lsp` uses. I created a autocmd to source global venv for python files and fixes the issue but i think `ty` must include the options for include or exclude or extra-paths for settings part in `vim.lsp.config`. Thanks btw. 

---

_Comment by @MichaReiser on 2025-10-01 07:29_

You'll need a [`ty.toml`](https://docs.astral.sh/ty/configuration/) configuration to set `extra-paths` or `environment.path`. We plan on supporting more options directly in the server in a future version. The configuration must be in the directory that you start the ty server instance

---

_Comment by @Dan7h3x on 2025-10-01 07:32_

Yeah, thanks i saw it in docs but its not `vim` way, but until those features gonna add to `ty` i will do `ty.toml` way. BTW `ty` is very good lsp thanks for developing it.

---

_Comment by @Dan7h3x on 2025-10-01 10:42_

In addition the `ty` doesn't provide autocompletions for arguments of functions/class

```py
def test(x:int,y:float):
    return x*y
```
The expected to have entries for `x` and `y`

Editor: Neovim
AutoComplete engine: blink-cmp

---

_Comment by @MichaReiser on 2025-10-01 11:04_

Auto completion in `test` does work in VS Code and I can't really help without knowing what your neovim configuration is

---

_Comment by @Dan7h3x on 2025-10-01 11:06_

My config is the default for `ty` in the docs.

---

_Comment by @sharkdp on 2025-10-01 14:22_

where exactly is your cursor placed? do you get completions in other cases?

---

_Comment by @Dan7h3x on 2025-10-01 14:34_

inside of a function parentheses and already wrote a know argument, and yes for other cases i got completion.

---

_Comment by @sharkdp on 2025-10-01 14:50_

> inside of a function parentheses and already wrote a know argument, and yes for other cases i got completion.

I don't understand. Is your cursor here:
```py
def test(x:int,y:float <CURSOR>):
    return x*y
```
or here?
```py
def test(x:int,y:float <CURSOR>):
    <CURSOR>
    return x*y
```

For the latter case, I do get completions in neovim:

<img width="639" height="296" alt="Image" src="https://github.com/user-attachments/assets/1fa495f1-c1b5-45df-8aff-064c883305ae" />

Does it work with longer parameter names? Does it work if you explicitly request completions? If so, you may want to configure completions on every keypress? I use this:

```lua
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
```

---

_Comment by @Dan7h3x on 2025-10-01 14:59_

sorry, my bad. The issue happens when you calling functions argument outside of function structure

```py
def test(x:int,y:int):
    return x*y

test(<cursor>)
```
in this situation i don't have entries for arguments in `cmp` and other lsps have.

---

_Comment by @sharkdp on 2025-10-01 15:24_

> when you calling functions

oh! I believe this is already tracked under "Identify function call sites and offer keyword based completions" in #86, so I'm going to close this now.


---

_Closed by @sharkdp on 2025-10-01 15:24_

---

_Comment by @Dan7h3x on 2025-10-01 15:30_

@sharkdp Ok, i didn't notice that.

---
