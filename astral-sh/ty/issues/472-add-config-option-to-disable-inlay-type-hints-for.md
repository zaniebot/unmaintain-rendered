```yaml
number: 472
title: Add config option to disable inlay type hints for variable assignment targets
type: issue
state: closed
author: pirate
labels:
  - server
assignees: []
created_at: 2025-05-20T01:11:34Z
updated_at: 2025-08-07T13:46:52Z
url: https://github.com/astral-sh/ty/issues/472
synced_at: 2026-01-12T15:54:23Z
```

# Add config option to disable inlay type hints for variable assignment targets

---

_@pirate_

I just installed the extension and was surprised to find it's trying to suggest inline type hints for a bunch of assignemnt expressions that don't need them:

<img width="387" alt="Image" src="https://github.com/user-attachments/assets/56f9c0cd-f6b9-4896-80f9-710d845ce756" />

<img width="390" alt="Image" src="https://github.com/user-attachments/assets/f36bc86e-a075-4827-983c-e36afe735e73" />

<img width="550" alt="Image" src="https://github.com/user-attachments/assets/e293847c-ca4a-4597-8d47-67ce9b946865" />

<img width="1145" alt="Image" src="https://github.com/user-attachments/assets/0f4528d3-3497-4c47-8f26-c6a82569816c" />

None of these are suggested by `pyright` or `mypy` LSP servers, and there doesn't seem to be a way to disable these suggestions.

---

_Comment by @dhruvmanila on 2025-05-21 14:16_

I agree! Thanks for reporting.

I'll move this to ty repository as it's related to the language server.

For reference, there is an option inside VS Code that's used by Pyright / Pylance:
```jsonc
  // Enable/disable inlay hints for variable types. Hints are not displayed for assignments of literals or constants:
  // ```python
  // foo':list[str]' = ["a"]
  // ```
  "python.analysis.inlayHints.variableTypes": false,
```

---

_Label `server` added by @dhruvmanila on 2025-05-21 14:16_

---

_Comment by @hahuang65 on 2025-05-21 21:25_

I'm not sure this will be a use-case in the future, but I use both `basedpyright` and `ty` in Neovim right now, as their feature sets don't fully overlap (yet?)

However, this causes an issue where if both try to set inlay hints, they trip over each other and actually throw errors like this:

```
Error in decoration provider vim_lsp_inlayhint.win:                                                                 
Error executing lua: /usr/share/nvim/runtime/lua/vim/lsp/inlay_hint.lua:345: Invalid 'col': out of range            
stack traceback:                                                                                                    
        [C]: in function 'nvim_buf_set_extmark'                                                                     
        /usr/share/nvim/runtime/lua/vim/lsp/inlay_hint.lua:345: in function </usr/share/nvim/runtime/lua/vim/lsp/inlay_hint.lua:311> 
```

This [reddit post](https://www.reddit.com/r/neovim/comments/1axlen1/inlay_hints_invalid_col_out_of_range/) is what led me to realize it was because I had both `basedpyright` and `ty` on.

Any way, with basedpyright, there is a granular setting for inlay hint types. If ty will be doing different inlay hint types as well, perhaps a granular option set like this would be worth consider?

```
      settings = {
        basedpyright = {
          analysis = {
            inlayHints = {
              variableTypes = false, -- conflicts with ty
              callArgumentNames = false, -- conflicts with ty
              functionReturnTypes = true,
              genericTypes = false, -- conflicts with ty
            },
          },
        },
      },
```

^
I've had to disable anything that MIGHT also have inlay hinting on the same line as a `ty` hint.

---

_Comment by @dhruvmanila on 2025-06-01 11:42_

Related: https://github.com/astral-sh/ty-vscode/issues/20

---

_Closed by @dhruvmanila on 2025-08-07 13:46_

---
