```yaml
number: 10065
title: Allow © in copyright notices
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cpy
created_at: 2024-02-20T16:39:57Z
updated_at: 2024-02-22T17:44:23Z
url: https://github.com/astral-sh/ruff/pull/10065
synced_at: 2026-01-10T22:57:10Z
```

# Allow © in copyright notices

---

_Pull request opened by @charliermarsh on 2024-02-20 16:39_

Closes https://github.com/astral-sh/ruff/issues/10061.

---

_Label `bug` added by @charliermarsh on 2024-02-20 16:40_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-20 16:59_

---

_Comment by @github-actions[bot] on 2024-02-20 17:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-02-20 18:07_

@BurntSushi - Is there a cost to making the pattern non-ASCII here?

---

_@BurntSushi approved on 2024-02-20 19:56_

LGTM. It is rather unlikely that this introduces any additional cost. Since the regex starts with a literal (even though it's case insensitive), this is probably getting optimized via a nice prefix prefilter. The best kind. What comes after won't matter much, and the regex is already matching non-ASCII things. For example, `\d` matches `𝟜`.

---

_Merged by @charliermarsh on 2024-02-22 17:44_

---

_Closed by @charliermarsh on 2024-02-22 17:44_

---

_Branch deleted on 2024-02-22 17:44_

---
