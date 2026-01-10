```yaml
number: 14601
title: "[`pylint`] Do not wrap function calls in parentheses in the fix for unnecessary-dunder-call (PLC2801)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: dunder-parens
created_at: 2024-11-26T05:18:47Z
updated_at: 2024-11-26T12:47:02Z
url: https://github.com/astral-sh/ruff/pull/14601
synced_at: 2026-01-10T20:50:57Z
```

# [`pylint`] Do not wrap function calls in parentheses in the fix for unnecessary-dunder-call (PLC2801)

---

_Pull request opened by @dylwil3 on 2024-11-26 05:18_

In the fix for [unnecessary-dunder-call (PLC2801)](https://docs.astral.sh/ruff/rules/unnecessary-dunder-call/#unnecessary-dunder-call-plc2801), when replacing the dunder call for a builtin by the function call, we no longer wrap the function call in parentheses.

Closes #14597


---

_Comment by @github-actions[bot] on 2024-11-26 05:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`pylint`] Do not wrap function calls in parentheses in the fix for dict-index-missing-items (PLC0206)" to "[`pylint`] Do not wrap function calls in parentheses in the fix for unnecessary-dunder-call (PLC2801)" by @dylwil3 on 2024-11-26 05:42_

---

_Label `bug` added by @MichaReiser on 2024-11-26 07:10_

---

_Label `fixes` added by @MichaReiser on 2024-11-26 07:10_

---

_@MichaReiser approved on 2024-11-26 07:20_

---

_Merged by @dylwil3 on 2024-11-26 12:47_

---

_Closed by @dylwil3 on 2024-11-26 12:47_

---
