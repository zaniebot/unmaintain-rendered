```yaml
number: 12011
title: "Remove `check`, `--explain`, `--clean`, `--generate-shell-completion` aliases"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - cli
assignees: []
merged: true
base: ruff-0.5
head: remove-CLI-aliases
created_at: 2024-06-24T09:30:12Z
updated_at: 2024-06-25T07:23:21Z
url: https://github.com/astral-sh/ruff/pull/12011
synced_at: 2026-01-12T15:55:40Z
```

# Remove `check`, `--explain`, `--clean`, `--generate-shell-completion` aliases

---

_@MichaReiser_

## Summary

This PR changes  `ruff <path>`, `ruff --explain`, `ruff --clean` and `ruff --generate-shell-completion` to error but continues to print an error message. The commands were deprecated in https://github.com/astral-sh/ruff/pull/10169 (v0.3.0)

Part of https://github.com/astral-sh/ruff/issues/10171


## Test Plan

I verified that:

* The JetBrains extension uses `ruff check`
* The LSP uses `ruff check`
* [python-lsp-ruff](https://github.com/python-lsp/python-lsp-ruff/blob/34acd6ed6daaaa3f2756a1a1cf1bfc2d76df7d0f/pylsp_ruff/plugin.py#L67) I think it uses `ruff check`

<!-- How was it tested? -->


---

_Label `breaking` added by @MichaReiser on 2024-06-24 09:30_

---

_Label `cli` added by @MichaReiser on 2024-06-24 09:30_

---

_Comment by @github-actions[bot] on 2024-06-24 09:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-06-24 10:07_

---

_Comment by @dhruvmanila on 2024-06-24 10:18_

I'm looking at the Neovim ecosystem, already completed in `conform.nvim`:  https://github.com/stevearc/conform.nvim/pull/348

---

_@charliermarsh approved on 2024-06-24 10:50_

---

_@zanieb reviewed on 2024-06-24 13:41_

---

_Review comment by @zanieb on `docs/configuration.md`:532 on 2024-06-24 13:41_

I feel like we've kept this hidden previously? Definitely nicer indentation when hidden.

---

_@MichaReiser reviewed on 2024-06-24 15:55_

---

_Review comment by @MichaReiser on `docs/configuration.md`:532 on 2024-06-24 15:55_

Nice catch. Yeah, I deleted a little too much. Thank you

---

_Comment by @MichaReiser on 2024-06-25 07:03_

I changed the implementation to error instead of removing the aliases entirely. 

---

_Renamed from "Remove `check`, `--explain`, `--clean`, `--generate-shell-completion` aliases`" to "Remove `check`, `--explain`, `--clean`, `--generate-shell-completion` aliases" by @MichaReiser on 2024-06-25 07:23_

---

_Merged by @MichaReiser on 2024-06-25 07:23_

---

_Closed by @MichaReiser on 2024-06-25 07:23_

---

_Branch deleted on 2024-06-25 07:23_

---
