```yaml
number: 2195
title: How to avoid doubled diagnostics ruff vs. ty?
type: issue
state: closed
author: jamilraichouni
labels:
  - question
  - server
assignees: []
created_at: 2025-12-23T22:51:50Z
updated_at: 2025-12-24T05:27:27Z
url: https://github.com/astral-sh/ty/issues/2195
synced_at: 2026-01-12T15:54:26Z
```

# How to avoid doubled diagnostics ruff vs. ty?

---

_@jamilraichouni_

### Question

Using Neovim v0.11.5 and `ruff` v0.14.10 I get this:

<img width="1357" height="411" alt="Image" src="https://github.com/user-attachments/assets/2975e858-b1c8-4171-8551-e67e43dd195b" />

My config is at the top right hand side and I marked the diagnostic error message which appears twice.

I looked at https://github.com/astral-sh/ty/blob/8c54a1cca2c41414bd4bc56c3b7b0591007f6d0f/docs/reference/editor-settings.md and also scanned the issues first.

Many thanks,
Jamil

### Version

0.0.6

---

_Label `question` added by @jamilraichouni on 2025-12-23 22:51_

---

_Label `server` added by @AlexWaygood on 2025-12-23 22:59_

---

_Comment by @carljm on 2025-12-23 23:59_

I'm more familiar with the ty side than the ruff side, but on the ty side you can just disable the `invalid-syntax` rule if you are also running ruff, e.g. in your `pyproject.toml`:

```toml
[tool.ty.rules]
invalid-syntax = "ignore"
```

---

_Comment by @AlexWaygood on 2025-12-24 00:06_

> I'm more familiar with the ty side than the ruff side, but on the ty side you can just disable the `invalid-syntax` rule if you are also running ruff, e.g. in your `pyproject.toml`:
> 
> [tool.ty.rules]
> invalid-syntax = "ignore"

I don't think that's true @carljm. A decision was made that `invalid-syntax` should be different from all other error codes and should not be suppressable via the usual mechanism.

There is however a ruff setting to switch off reporting of syntax errors in the editor specifically: https://docs.astral.sh/ruff/editors/settings/#showsyntaxerrors.

And there is an open issue to add the same setting for ty: https://github.com/astral-sh/ty/issues/2106

---

_Comment by @jamilraichouni on 2025-12-24 00:11_

Thanks!

Tried that via

```lua
vim.lsp.config("ty", {
    cmd = { "ty", "server" },
    filetypes = { "python" },
    root_markers = { "pyproject.toml", ".git", "ty.toml" },
    settings = {
        -- https://github.com/astral-sh/ty/blob/main/docs/reference/editor-settings.md
        ty = {
            disableLanguageServices = true,
            configuration = {
                rules = {
                    ["invalid-syntax"] = "ignore",
                },
            }
        }
    }
})
```

and the diagnostic still appears.
#2106 sounds like what I need.

---

_Comment by @carljm on 2025-12-24 00:14_

Seems like you could probably set `showSyntaxErrors` to `false` in your Ruff LSP config today, if you don't want to wait for #2106 in ty?


---

_Closed by @dhruvmanila on 2025-12-24 05:27_

---
