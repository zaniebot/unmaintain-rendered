```yaml
number: 17977
title: Update editor integrations link in README
type: pull_request
state: merged
author: otakutyrant
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-05-09T09:35:23Z
updated_at: 2025-05-26T08:50:09Z
url: https://github.com/astral-sh/ruff/pull/17977
synced_at: 2026-01-12T15:56:09Z
```

# Update editor integrations link in README

---

_@otakutyrant_

Especially the wrong link of "editor integrations".

---

_@oscargus reviewed on 2025-05-09 10:47_

---

_Review comment by @oscargus on `README.md`:37 on 2025-05-09 10:47_

```suggestion
- ⌨️ First-party integrations for [VS Code](https://github.com/astral-sh/ruff-vscode) or [other editors](https://docs.astral.sh/ruff/editors) like [Neovim](https://docs.astral.sh/ruff/editors/setup/#neovim), and [more development automation and environment management tools]([https://docs.astral.sh/ruff/editors/setup](https://docs.astral.sh/ruff/integrations)
```

---

_Label `documentation` added by @dylwil3 on 2025-05-11 20:25_

---

_@MichaReiser reviewed on 2025-05-12 07:35_

---

_Review comment by @MichaReiser on `README.md`:37 on 2025-05-12 07:35_

I don't think it's correct to call these first-party integration:

* neovim: The neovim plugin isn't maintained by us
* other editors: The same is true for other editors, e.g. the zed extension is built by someone else
* We also don't provide first-party support for all integrations listed under integrations.

---

_Comment by @MichaReiser on 2025-05-12 07:36_

Thanks, I think we should just fix the link for the VS Code editor integration

---

_Renamed from "refine the first-party integrations description" to "Update editor integrations link in README" by @MichaReiser on 2025-05-26 07:48_

---

_Comment by @github-actions[bot] on 2025-05-26 08:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2025-05-26 08:50_

---

_Closed by @MichaReiser on 2025-05-26 08:50_

---
