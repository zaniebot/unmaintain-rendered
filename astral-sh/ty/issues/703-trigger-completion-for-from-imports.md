```yaml
number: 703
title: Trigger completion for from-imports
type: issue
state: closed
author: twoertwein
labels:
  - question
  - server
  - completions
assignees: []
created_at: 2025-06-25T16:14:37Z
updated_at: 2025-10-03T12:19:37Z
url: https://github.com/astral-sh/ty/issues/703
synced_at: 2026-01-12T15:54:23Z
```

# Trigger completion for from-imports

---

_@twoertwein_

### Summary

The completion is already working very well :)

I noticed that while https://play.ty.dev triggers autocompletion even after the first letter of a from-import (for example `from pathlib import P<courser>`). The autocompletion in neovim is only triggered after the trigger character specified by ty (only "." https://github.com/astral-sh/ruff/blob/8b22992988226833da6804e79fef4706fb657ead/playground/ty/src/Editor/Editor.tsx#L182). Manually requesting the completion at `from pathlib import P<courser>` works :)

It would be great to extend the autotrigger options (or provide a neovim example config that closely mirrors the behavior of play.ty.dev)

### Version

0.0.1 alpha 12

---

_Label `server` added by @AlexWaygood on 2025-06-25 16:17_

---

_Assigned to @BurntSushi by @AlexWaygood on 2025-06-25 16:17_

---

_Comment by @dhruvmanila on 2025-06-26 03:58_

Can you provide us with your Neovim auto-completion setup? Like, are you using a plugin for that or the builtin completion engine? I'm seeing the completion menu popup for the example that you've provided:

https://github.com/user-attachments/assets/43a03b9e-6e05-49dd-b0a1-c42186d8211c

---

_Comment by @twoertwein on 2025-06-26 12:21_

I'm using the builtin completion, minimal example:


init.lua
```lua
vim.lsp.enable { "ty" }

vim.cmd[[set completeopt+=menuone,noselect,popup]]
```

lsp/ty.lua
```lua
return {
  cmd = { "ty", "server" },
  filetypes = { "python" },
  root_markers = { "ty.toml", "pyproject.toml", ".git" },
  on_attach = function(client, bufnr)
    -- show type annotations
    vim.lsp.inlay_hint.enable(true, {  bufnr })
    -- automatically trigger completions
    vim.lsp.completion.enable(true, client.id, bufnr, {
      autotrigger = true,
      convert = function(item)
        return { abbr = item.label:gsub("%b()", "") }
      end,
    })
  end,
}
```


---

_Comment by @dhruvmanila on 2025-06-27 06:20_

Thank you for providing the details!

I think this might be a limitation on Neovim side because as per the docs of [`vim.lsp.completions`](https://neovim.io/doc/user/lsp.html#lsp-completion), it only uses the trigger characters defined by the server and does not use any pre-existing trigger characters. Most clients usually define the set of alphabets `[a-zA-Z]` as the default trigger characters and then extend that with the one defined by the server. As per the [LSP spec](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionOptions), the `triggerCharacters` field is mainly use to _extend_ the characters defined by the client and not re-define them.

Can you try opening an issue with Neovim and check-in if they're open to having a default set of trigger characters?

---

_Unassigned @BurntSushi by @AlexWaygood on 2025-06-27 07:38_

---

_Comment by @twoertwein on 2025-06-27 15:05_

Thank you for linking the docs! Neovim actually has an [example](https://neovim.io/doc/user/lsp.html#lsp-attach) to trigger LSP on every keypress :) (luckily ty is fast)

---

_Closed by @twoertwein on 2025-06-27 15:05_

---

_Label `question` added by @dhruvmanila on 2025-06-30 03:00_

---

_Comment by @dhruvmanila on 2025-06-30 03:05_

> Thank you for linking the docs! Neovim actually has an [example](https://neovim.io/doc/user/lsp.html#lsp-attach) to trigger LSP on every keypress :) (luckily ty is fast)

That's great! I guess then you can define the `[a-zA-Z]` trigger character similar to the example in the docs:

```lua
-- Start with the characters provided by the server
local chars = client.server_capabilities.completionProvider.triggerCharacters

-- Extend it with the uppercase and lowercase alphabets
for i = 65, 90 do  -- A - Z
	table.insert(chars, string.char(i))
end
for i = 97, 122 do  -- a - z
	table.insert(chars, string.char(i))
end

client.server_capabilities.completionProvider.triggerCharacters = chars
```

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---
