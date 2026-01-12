```yaml
number: 12448
title: Ignore more open ai notebooks for now
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: openai-more-ecosystem-exclude
created_at: 2024-07-22T07:55:30Z
updated_at: 2024-07-22T12:16:50Z
url: https://github.com/astral-sh/ruff/pull/12448
synced_at: 2026-01-12T15:55:41Z
```

# Ignore more open ai notebooks for now

---

_@MichaReiser_

_No description provided._

---

_Renamed from "\" to "Ignore more open ai notebooks for now" by @MichaReiser on 2024-07-22 07:58_

---

_Comment by @MichaReiser on 2024-07-22 07:59_

These notebooks contain `code` cells with `vscode.languageId` set to a language other than Python. Ruff doesn't understand `vscode.languageId` yet and will incorrectly assume these are python cells (and thus, fail parsing). 

See https://github.com/astral-sh/ruff/issues/12281

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-22 07:59_

---

_Label `ci` added by @MichaReiser on 2024-07-22 07:59_

---

_Comment by @github-actions[bot] on 2024-07-22 08:14_

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

_@dhruvmanila approved on 2024-07-22 08:55_

---

_Merged by @MichaReiser on 2024-07-22 12:16_

---

_Closed by @MichaReiser on 2024-07-22 12:16_

---

_Branch deleted on 2024-07-22 12:16_

---
