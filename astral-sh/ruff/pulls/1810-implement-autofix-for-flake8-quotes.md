```yaml
number: 1810
title: Implement autofix for flake8-quotes
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: autofix-flake8-quotes
created_at: 2023-01-12T08:34:08Z
updated_at: 2023-01-13T01:05:00Z
url: https://github.com/astral-sh/ruff/pull/1810
synced_at: 2026-01-12T15:55:07Z
```

# Implement autofix for flake8-quotes

---

_@messense_

Resolves #1789

---

_Review comment by @charliermarsh on `src/flake8_quotes/rules.rs`:88 on 2023-01-12 15:50_

Do we need to handle escapes here? E.g., when converting this to double quotes:

```py
'Here's my content " with a double quote in the middle'
```

I'm guessing _no_, because we'd actually consider that acceptable (and thus not needing to be changed) based on the existing logic.


---

_@charliermarsh reviewed on 2023-01-12 15:50_

---

_Merged by @charliermarsh on 2023-01-12 17:42_

---

_Closed by @charliermarsh on 2023-01-12 17:42_

---

_Branch deleted on 2023-01-13 01:05_

---
