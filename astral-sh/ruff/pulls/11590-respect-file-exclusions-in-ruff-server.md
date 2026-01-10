```yaml
number: 11590
title: "Respect file exclusions in `ruff server`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: charlie/exclude-lsp
created_at: 2024-05-29T00:45:40Z
updated_at: 2024-05-29T03:08:25Z
url: https://github.com/astral-sh/ruff/pull/11590
synced_at: 2026-01-10T21:56:00Z
```

# Respect file exclusions in `ruff server`

---

_Pull request opened by @charliermarsh on 2024-05-29 00:45_

## Summary

Closes https://github.com/astral-sh/ruff/issues/11587.

## Test Plan

- Added a lint error to `test_server.py` in `vscode-ruff`.
- Validated that, prior to this change, diagnostics appeared in the file.
- Validated that, with this change, no diagnostics were shown.
- Validated that, with this change, no diagnostics were fixed on-save.


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-29 00:45_

---

_Label `bug` added by @charliermarsh on 2024-05-29 00:45_

---

_Label `server` added by @charliermarsh on 2024-05-29 00:45_

---

_@charliermarsh reviewed on 2024-05-29 00:46_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/format.rs`:110 on 2024-05-29 00:46_

I intentionally did _not_ add this to the range format action. That feels intentional enough to me that it's fine?

---

_Comment by @github-actions[bot] on 2024-05-29 01:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-05-29 02:10_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:110 on 2024-05-29 02:10_

I think we might want to add it to range formatting as well. Otherwise, Ruff will always format when auto-format is enabled for modifications in VS Code even if the file is excluded.
```json
{
  "editor.formatOnSave": true,
  "editor.formatOnSaveMode": "modifications"
}
```

---

_@dhruvmanila approved on 2024-05-29 02:13_

---

_@charliermarsh reviewed on 2024-05-29 02:27_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/format.rs`:110 on 2024-05-29 02:27_

Ok, sounds good.

---

_Merged by @charliermarsh on 2024-05-29 02:58_

---

_Closed by @charliermarsh on 2024-05-29 02:58_

---

_Branch deleted on 2024-05-29 02:58_

---
