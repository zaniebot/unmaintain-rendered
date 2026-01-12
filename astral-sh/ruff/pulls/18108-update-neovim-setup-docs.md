```yaml
number: 18108
title: Update Neovim setup docs
type: pull_request
state: merged
author: bombsimon
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-nvim-docs
created_at: 2025-05-14T19:32:12Z
updated_at: 2025-05-14T20:58:03Z
url: https://github.com/astral-sh/ruff/pull/18108
synced_at: 2026-01-12T15:56:12Z
```

# Update Neovim setup docs

---

_@bombsimon_

Nvim 0.11+ uses the builtin `vim.lsp.enable` and `vim.lsp.config` to enable and configure LSP clients. This adds the new non legacy way of configuring Nvim with `nvim-lspconfig` according to the upstream documentation.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Update documentation for Nvim LSP configuration according to `nvim-lspconfig` and Nvim 0.11+

## Test Plan

Tested locally on macOS with Nvim 0.11.1 and `nvim-lspconfig` master/[ac1dfbe](https://github.com/neovim/nvim-lspconfig/tree/ac1dfbe3b60e5e23a2cff90e3bd6a3bc88031a57).

---

_@ntBre reviewed on 2025-05-14 19:55_

---

_Review comment by @ntBre on `docs/editors/setup.md`:39 on 2025-05-14 19:55_

We might want to use a sub-heading here, or at least indent the code blocks to sit under the bullet points.

I'm not personally very familiar with neovim, but how much of the other config code below this also needs to be updated? I see that much of it starts with `require('lspconfig')` too. The existing docs just above this also suggest installing `nvim-lspconfig`.

cc @dhruvmanila

---

_Label `documentation` added by @ntBre on 2025-05-14 19:55_

---

_Review requested from @dhruvmanila by @ntBre on 2025-05-14 19:56_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-14 20:00_

---

_Comment by @dhruvmanila on 2025-05-14 20:01_

Thanks! I'll play around with the structure a bit before merging but I agree with Brent about probably using a sub-heading here.

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:39 on 2025-05-14 20:03_

The new way of configuring doesn't require importing i.e., `require('lspconfig')` because `vim` is a builtin global variable much like `int` or `map` in Python.

---

_@dhruvmanila reviewed on 2025-05-14 20:03_

---

_@bombsimon reviewed on 2025-05-14 20:04_

---

_Review comment by @bombsimon on `docs/editors/setup.md`:39 on 2025-05-14 20:04_

> but how much of the other config code below this also needs to be updated?

I think this is up to you and depending on how long you want to document < 0.11. [`nvim-lspconfig`](https://github.com/neovim/nvim-lspconfig) already mentions the other format as deprecated and not supported but just landed in March so might be some time before users adopt.

I think no need to change all occurrences of `require('lspconfig').<lsp>.setup` (only 3 places) for now, I think if you're on 0.11 and set up ruff you know how to setup `pyright` to ignore files for analysis too.

> ... at least indent the code blocks to sit under the bullet points

Do'h, my bad! 🤦  Fixed this but feel free to move it around or use sub-headings!

---

_@dhruvmanila reviewed on 2025-05-14 20:13_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:39 on 2025-05-14 20:13_

Hmm, actually `nvim-lspconfig` would still be required here because the actual config definition is in that repository so the config is only partial here. Thanks @ntBre for pointing that out!

---

_@bombsimon reviewed on 2025-05-14 20:36_

---

_Review comment by @bombsimon on `docs/editors/setup.md`:39 on 2025-05-14 20:36_

Yeah I thought that was intentional since it's already mentioned above. Given all the variants of package managers and bundled Nvim installations it's hard to pick one way to install and setup `nvim-lspconfig`.

I think it's best to just keep the current text that refers to the docs on how to install and configure it.

---

_@dhruvmanila reviewed on 2025-05-14 20:47_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:39 on 2025-05-14 20:47_

Yeah, I noticed that a bit later, sorry! I've pushed a change to use [content tabs](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/) instead of header


---

_Renamed from "[docs] update Nvim LSP docs" to "Update Neovim setup docs" by @dhruvmanila on 2025-05-14 20:48_

---

_@dhruvmanila approved on 2025-05-14 20:52_

Thank you!

---

_Merged by @dhruvmanila on 2025-05-14 20:54_

---

_Closed by @dhruvmanila on 2025-05-14 20:54_

---

_Branch deleted on 2025-05-14 20:58_

---
