```yaml
number: 9919
title: "Use boxed `str` in `Identifier`"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/box-id
created_at: 2024-02-10T02:20:24Z
updated_at: 2024-03-28T17:29:40Z
url: https://github.com/astral-sh/ruff/pull/9919
synced_at: 2026-01-10T22:47:01Z
```

# Use boxed `str` in `Identifier`

---

_Pull request opened by @charliermarsh on 2024-02-10 02:20_

_No description provided._

---

_Comment by @github-actions[bot] on 2024-02-10 02:49_

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

_Comment by @charliermarsh on 2024-02-10 15:12_

Somehow this causes a significant parser regression for me locally (like 3%).

---

_Comment by @charliermarsh on 2024-02-10 15:13_

I don't understand why, since the parser is almost entirely unchanged.

---

_Comment by @MichaReiser on 2024-03-28 17:29_

I close this, assuming that you don't plan on landing this soon. Feel free to reopen if I misread your intentions. 

---

_Closed by @MichaReiser on 2024-03-28 17:29_

---
