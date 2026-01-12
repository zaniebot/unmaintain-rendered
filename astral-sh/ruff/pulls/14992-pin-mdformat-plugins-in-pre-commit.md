```yaml
number: 14992
title: Pin mdformat plugins in pre-commit
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: alex/pre-commit
created_at: 2024-12-15T19:33:21Z
updated_at: 2024-12-15T19:37:46Z
url: https://github.com/astral-sh/ruff/pull/14992
synced_at: 2026-01-12T15:55:49Z
```

# Pin mdformat plugins in pre-commit

---

_@AlexWaygood_

The latest version of mdformat-mkdocs apparently conflicts with our other Markdown pre-commit hook, markdownlint. I haven't spent too much time figuring out if there's a way to make the latest versions of the two happy with each other, since the conflict is causing pre-commit to fail on the main branch.

I tested locally that pre-commit passes on this branch.

---

_Label `internal` added by @AlexWaygood on 2024-12-15 19:34_

---

_Label `ci` added by @AlexWaygood on 2024-12-15 19:34_

---

_Merged by @AlexWaygood on 2024-12-15 19:37_

---

_Closed by @AlexWaygood on 2024-12-15 19:37_

---

_Branch deleted on 2024-12-15 19:37_

---
