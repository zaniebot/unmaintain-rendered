```yaml
number: 18324
title: Update editor setup docs about Neovim and Vim
type: pull_request
state: merged
author: otakutyrant
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-05-26T16:49:48Z
updated_at: 2025-06-06T06:39:30Z
url: https://github.com/astral-sh/ruff/pull/18324
synced_at: 2026-01-10T18:45:04Z
```

# Update editor setup docs about Neovim and Vim

---

_Pull request opened by @otakutyrant on 2025-05-26 16:49_

## Summary

I struggled to make ruff_organize_imports work and then I found out I missed the key note about conform.nvim before because it was put in the Vim section wrongly! So I refined them both.

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-05-26 16:50_

---

_Label `documentation` added by @AlexWaygood on 2025-05-26 20:17_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:145 on 2025-05-28 05:22_

We might as well use Lua here as it's specific to Neovim.

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:153 on 2025-05-28 05:22_

What do you mean by "especially about organizing imports order" ?

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:145 on 2025-05-28 05:25_

Not a big fan of duplicating these instructions for Vim and Neovim although not sure what's the best path would be to avoid the duplication.

What about just including the common setups in Neovim and highlighting the fact that this would work for Vim as well? What do you think?

---

_@dhruvmanila reviewed on 2025-05-28 05:25_

---

_@otakutyrant reviewed on 2025-05-28 05:29_

---

_Review comment by @otakutyrant on `docs/editors/setup.md`:153 on 2025-05-28 05:29_

Conform provides [a specific formatter about organizing imports order](https://github.com/stevearc/conform.nvim/blob/master/lua/conform/formatters/ruff_organize_imports.lua), as ruff does not select "I" at default.

---

_Review comment by @otakutyrant on `docs/editors/setup.md`:145 on 2025-05-28 05:32_

I have a strong preference for Neovim, as Vim may eventually fade into obsolescence. Thus, focusing primarily on Neovim with Vim as a secondary option would be most suitable.

---

_@otakutyrant reviewed on 2025-05-28 05:32_

---

_@otakutyrant reviewed on 2025-05-28 05:34_

---

_Review comment by @otakutyrant on `docs/editors/setup.md`:145 on 2025-05-28 05:34_

I confess I'm not well-versed in ale, nor do I have any inclination to incorporate its code.

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:153 on 2025-05-28 05:36_

Yes, and it's mentioned in the code snippet. I don't think it's worth highlighting that here.

---

_@dhruvmanila reviewed on 2025-05-28 05:36_

---

_@otakutyrant reviewed on 2025-05-28 06:24_

---

_Review comment by @otakutyrant on `docs/editors/setup.md`:153 on 2025-05-28 06:24_

Okay. But what do I do? Is it okay to delete the highlighting, git commit --amend and git push --force finally?

---

_@dhruvmanila reviewed on 2025-05-28 07:53_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:153 on 2025-05-28 07:53_

We always use "Squash and merge" so it's fine to create a new commit as well, whichever you prefer.

---

_@dhruvmanila approved on 2025-06-03 07:38_

Thank you! I made a couple of edits mainly around rearranging the ALE plugin section.

---

_Renamed from "[ruff] Update setup.md about Neovim and Vim" to "Update editor setup docs about Neovim and Vim" by @dhruvmanila on 2025-06-03 07:38_

---

_Merged by @dhruvmanila on 2025-06-03 07:40_

---

_Closed by @dhruvmanila on 2025-06-03 07:40_

---

_Comment by @BarrensZeppelin on 2025-06-06 06:39_

FWIW 3rd party plugins are not required, native LSP works as well with a config like this:

```lua
local ms = vim.lsp.protocol.Methods
vim.lsp.config("ruff", {
  on_attach = function(client, bufnr)
    -- Set a keymap to both ruff fix and ruff format
    vim.keymap.set("n", "<Leader>f", function()
      client:request_sync(ms.workspace_executeCommand, {
        command = "ruff.applyAutofix",
        arguments = { { uri = vim.uri_from_bufnr(bufnr), version = vim.lsp.util.buf_versions[bufnr] } },
      }, 1000, bufnr)
      vim.lsp.buf.format({
        async = true,
        bufnr = bufnr,
        id = client.id,
      })
    end, { buffer = bufnr, desc = "Format buffer with ruff" })
  end,
} --[[@as vim.lsp.Config]])
```

---
