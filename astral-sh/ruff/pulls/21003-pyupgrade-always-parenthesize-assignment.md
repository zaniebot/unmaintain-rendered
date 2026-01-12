```yaml
number: 21003
title: "[`pyupgrade`] Always parenthesize assignment expressions in fix for `f-string` (`UP032`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: up032-assignment-expr
created_at: 2025-10-20T18:16:06Z
updated_at: 2025-10-20T23:34:03Z
url: https://github.com/astral-sh/ruff/pull/21003
synced_at: 2026-01-12T15:57:14Z
```

# [`pyupgrade`] Always parenthesize assignment expressions in fix for `f-string` (`UP032`)

---

_@dylwil3_

Closes #21000 


---

_Label `bug` added by @dylwil3 on 2025-10-20 18:16_

---

_Label `fixes` added by @dylwil3 on 2025-10-20 18:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs`:169 on 2025-10-20 20:28_

It would be nice if we could use OperatorPrecedence here (I believe that's the name) instead of listing all expressions 

---

_@MichaReiser approved on 2025-10-20 20:28_

---

_@dylwil3 reviewed on 2025-10-20 20:38_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs`:169 on 2025-10-20 20:38_

Hm... it's not immediately clear how to do this here - especially because some of this logic is to avoid double escaped braces for sets and dicts, like `f"{{x}}"`

---

_Comment by @github-actions[bot] on 2025-10-20 23:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-10-20 23:34_

---

_Closed by @dylwil3 on 2025-10-20 23:34_

---
