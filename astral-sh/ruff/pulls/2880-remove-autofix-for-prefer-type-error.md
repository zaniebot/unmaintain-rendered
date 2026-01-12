```yaml
number: 2880
title: Remove autofix for prefer-type-error
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/prefer-type-error
created_at: 2023-02-14T02:24:03Z
updated_at: 2023-02-16T08:23:47Z
url: https://github.com/astral-sh/ruff/pull/2880
synced_at: 2026-01-12T04:39:44Z
```

# Remove autofix for prefer-type-error

---

_Pull request opened by @charliermarsh on 2023-02-14 02:24_

This isn't really safe, so let's remove it.

Closes #2878.

---

_Merged by @charliermarsh on 2023-02-14 02:26_

---

_Closed by @charliermarsh on 2023-02-14 02:26_

---

_Branch deleted on 2023-02-14 02:26_

---

_Comment by @sbrugman on 2023-02-16 08:23_

This could be re-enabled once autofix aggressiveness levels are implemented (https://github.com/charliermarsh/ruff/issues/1997). The fix often is the right fix, but we can't know that for sure automatically. The user needs to check if the fix is safe.

---
