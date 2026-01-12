```yaml
number: 11499
title: "Update documentation for `ruff server` with new migration guide"
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: jane/server/beta/doc-update
created_at: 2024-05-22T20:50:02Z
updated_at: 2024-05-22T21:38:11Z
url: https://github.com/astral-sh/ruff/pull/11499
synced_at: 2026-01-12T15:55:38Z
```

# Update documentation for `ruff server` with new migration guide

---

_@snowsignal_

## Summary

Introduces a migration guide from `ruff-lsp` to `ruff server` and makes small updates to the `README.md`.

---

_Label `documentation` added by @snowsignal on 2024-05-22 20:50_

---

_Label `server` added by @snowsignal on 2024-05-22 20:50_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-05-22 20:50_

---

_Review requested from @charliermarsh by @snowsignal on 2024-05-22 20:50_

---

_Review comment by @charliermarsh on `crates/ruff_server/README.md`:12 on 2024-05-22 20:52_

You should include a JSON snippet demonstrating the exact setting that you need to set, and to what value. People will ignore the text and look to copy-paste.

---

_@charliermarsh reviewed on 2024-05-22 20:52_

---

_Review comment by @charliermarsh on `crates/ruff_server/docs/MIGRATION.md`:44 on 2024-05-22 20:53_

I think we should include some before-and-after examples here for VS Code especially. E.g., if I was passing `--line-length` via `lint.args` before, what should I do now? I like the `--select` example at the end, but let's show the exact JSON before and after.

---

_@charliermarsh reviewed on 2024-05-22 20:54_

---

_@charliermarsh reviewed on 2024-05-22 21:12_

---

_Review comment by @charliermarsh on `crates/ruff_server/README.md`:23 on 2024-05-22 21:12_

Let's make this valid JSON, to match what we have here: https://github.com/astral-sh/ruff-vscode?tab=readme-ov-file#enabling-the-rust-based-language-server.

---

_Review requested from @charliermarsh by @snowsignal on 2024-05-22 21:18_

---

_Review comment by @charliermarsh on `crates/ruff_server/docs/MIGRATION.md`:7 on 2024-05-22 21:22_

"The VS Code extension settings have documentation that will tell you whether `ruff server` supports a given setting"

---

_@charliermarsh reviewed on 2024-05-22 21:22_

---

_@charliermarsh approved on 2024-05-22 21:35_

---

_Merged by @snowsignal on 2024-05-22 21:36_

---

_Closed by @snowsignal on 2024-05-22 21:36_

---

_Branch deleted on 2024-05-22 21:36_

---

_Comment by @github-actions[bot] on 2024-05-22 21:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
