```yaml
number: 436
title: Add setup guide for using language server in Neovim
type: pull_request
state: merged
author: fabridamicelli
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-neovim-start-docs
created_at: 2025-05-17T19:41:47Z
updated_at: 2025-08-16T06:27:38Z
url: https://github.com/astral-sh/ty/pull/436
synced_at: 2026-01-10T02:34:10Z
```

# Add setup guide for using language server in Neovim

---

_Pull request opened by @fabridamicelli on 2025-05-17 19:41_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Thanks for this tool!

This config worked right away for me for type checking, so at least until the docs are more exhaustive, we could have this snippet.

## Summary
Add documentation for minimal setup of `ty` on Neovim.

## Test Plan
This worked out-of-the-box for me

![image](https://github.com/user-attachments/assets/f8a43999-7351-43fd-939e-1a5ff0d9f9d1)

<!-- How was it tested? -->


---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-05-17 20:08_

---

_Label `documentation` added by @AlexWaygood on 2025-05-17 20:08_

---

_Comment by @hahuang65 on 2025-05-18 00:35_

FWIW, I've found that even if you don't have `ty` installed (because the mason PR hasn't been accepted yet), you can still run `{ "uvx", "ty", "server" }` and get it to work, if you have `uv` installed.

In fact, I personally even run it with `poetry` if I discover a `poetry.lock` file, in order to automatically get the venv loaded up for the LSP.

```
      if require("util").dir_has_file(root_dir, "poetry.lock") then
        vim.notify_once("Running `ty` with `poetry`")
        cmd = { "poetry", "run", "uvx", "ty", "server" }
      else
        vim.notify_once("Running `ty` without a virtualenv")
        cmd = { "uvx", "ty", "server" }
      end
```

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:162 on 2025-05-18 16:01_

I don't think this is required given that the plugin includes the ty config: https://github.com/neovim/nvim-lspconfig/blob/ac1dfbe3b60e5e23a2cff90e3bd6a3bc88031a57/lsp/ty.lua#L4



---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:135 on 2025-05-18 16:50_

This section should actually be present under the https://github.com/astral-sh/ty/blob/main/docs/README.md#other-editors section instead. This document is only to provide reference for all editor settings.

---

_@dhruvmanila requested changes on 2025-05-18 16:54_

I'd suggest moving the section to the relevant page and use https://docs.astral.sh/ruff/editors/setup/#neovim as a reference.

---

_@fabridamicelli reviewed on 2025-05-18 19:04_

---

_Review comment by @fabridamicelli on `docs/reference/editor-settings.md`:162 on 2025-05-18 19:04_

You are right. Now it is correct and follows instructions as in Ruff.

---

_@fabridamicelli reviewed on 2025-05-18 19:04_

---

_Review comment by @fabridamicelli on `docs/reference/editor-settings.md`:135 on 2025-05-18 19:04_

done

---

_Review requested from @dhruvmanila by @fabridamicelli on 2025-05-18 19:06_

---

_Renamed from "docs: add how to start server in neovim" to "Add setup guide for using language server in Neovim" by @dhruvmanila on 2025-05-21 15:21_

---

_@dhruvmanila approved on 2025-05-21 15:22_

Thank you!

---

_Merged by @dhruvmanila on 2025-05-21 15:23_

---

_Closed by @dhruvmanila on 2025-05-21 15:23_

---

_Comment by @Jaehaks on 2025-08-15 00:24_

How ot configure settings in neovim? 

I wrote like this, but ty cannot recognize the rule.
or can I set config file location like ruff? ruff lsp supports `settings.configuration` to set config file globally

```lua
vim.lsp.config('ty', {
  cmd = {'ty', 'server'},
  filetypes = {'python'},
  root_dir = function (bufnr, cb)
    local root = vim.fs.root(bufnr, {
      'ty.toml',
      'pyproject.toml',
      '.git'
    }) or vim.fn.getcwd()
    -- root directory must be transferred to callback function to recognize
    cb(root)
  end,
  settings = { -- see https://docs.astral.sh/ty/reference/configuration/
    ty = {
      rules = {
        ['division-by-zero'] = 'error',
      }
    },
  },
})

```

---

_Comment by @dhruvmanila on 2025-08-15 02:16_

Currently, ty server only supports the settings mentioned in https://docs.astral.sh/ty/reference/editor-settings/ to be defined directly in an editor, we plan to add more in the future as per user feedback. I'd suggest you open a new issue to request a specific setting to be available directly in an editor if you wish so.

Regardless, you can use a configuration file (`ty.toml` or `pyproject.toml`) to specify other available settings and the server should pick that up unless it's outside the project itself.

Related: https://github.com/astral-sh/ty/issues/786

---

_Comment by @Jaehaks on 2025-08-16 06:27_

@dhruvmanila 
Thanks a lot for your reply:)

---
