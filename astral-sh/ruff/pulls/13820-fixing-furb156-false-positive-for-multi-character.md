```yaml
number: 13820
title: Fixing FURB156 false positive for multi-character string error
type: pull_request
state: closed
author: Aditya-PS-05
labels:
  - bug
  - rule
  - preview
assignees: []
base: main
head: bug/13802-furb156-false-positive
created_at: 2024-10-19T13:49:45Z
updated_at: 2024-11-09T02:35:24Z
url: https://github.com/astral-sh/ruff/pull/13820
synced_at: 2026-01-12T15:55:45Z
```

# Fixing FURB156 false positive for multi-character string error

---

_@Aditya-PS-05_

closes #13802 

---

_Comment by @github-actions[bot] on 2024-10-19 14:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2024-10-19 14:22_

The set-like replacement is also safe when the left operand is an empty string. Actually, to be precise, it’s safe when the left operand is a substring of both the original right operand and the replacement right operand, or a substring of neither; for example, `"01" in "9012345678"` can safely become `"01" in string.digits`, and `"xyz" in "0123456789"` can safely become `"xyz" in string.digits`.

---

_@MichaReiser requested changes on 2024-10-21 08:47_

Could you please add some tests demonstrating what behavior this PR is changing and that it works as expected. 

---

_Label `bug` added by @MichaReiser on 2024-10-21 08:47_

---

_Label `rule` added by @MichaReiser on 2024-10-21 08:47_

---

_Label `preview` added by @MichaReiser on 2024-10-21 08:47_

---

_Closed by @charliermarsh on 2024-11-09 02:35_

---
