```yaml
number: 16054
title: "[`ruff`] Skip type definitions for `missing-f-string-syntax` (`RUF027`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: no-add-fstring-type-def
created_at: 2025-02-09T16:09:18Z
updated_at: 2025-02-09T16:16:29Z
url: https://github.com/astral-sh/ruff/pull/16054
synced_at: 2026-01-10T19:57:22Z
```

# [`ruff`] Skip type definitions for `missing-f-string-syntax` (`RUF027`)

---

_Pull request opened by @dylwil3 on 2025-02-09 16:09_

As an f-string is never correct in a type definition context, we skip [missing-f-string-syntax (RUF027)](https://docs.astral.sh/ruff/rules/missing-f-string-syntax/#missing-f-string-syntax-ruf027) in this case.

Closes #16037 


---

_Label `bug` added by @dylwil3 on 2025-02-09 16:09_

---

_Label `rule` added by @dylwil3 on 2025-02-09 16:09_

---

_Label `preview` added by @dylwil3 on 2025-02-09 16:09_

---

_@AlexWaygood approved on 2025-02-09 16:14_

Excellent, thank you! We _could_ consider adding an integration test in https://github.com/astral-sh/ruff/blob/main/crates/ruff/tests/lint.rs that runs TC006 in combination with RUF027 on this snippet. But it's probably not really necessary here

---

_Comment by @dylwil3 on 2025-02-09 16:16_

>We could consider adding an integration test in https://github.com/astral-sh/ruff/blob/main/crates/ruff/tests/lint.rs that runs TC006 in combination with RUF027 on this snippet. But it's probably not really necessary here

Yeah, I think it's probably okay. FWIW I tested it manually.

---

_Comment by @github-actions[bot] on 2025-02-09 16:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-02-09 16:16_

---

_Closed by @dylwil3 on 2025-02-09 16:16_

---
