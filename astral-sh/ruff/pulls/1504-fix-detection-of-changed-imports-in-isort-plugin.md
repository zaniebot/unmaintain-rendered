```yaml
number: 1504
title: Fix detection of changed imports in isort plugin
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: fix-converge-issues-with-isort-and-line-endings
created_at: 2022-12-31T08:16:15Z
updated_at: 2022-12-31T12:43:07Z
url: https://github.com/astral-sh/ruff/pull/1504
synced_at: 2026-01-12T15:55:06Z
```

# Fix detection of changed imports in isort plugin

---

_@squiddy_

With the correct handling of line endings recently introduced, the isort plugin got into an endless loop because the check whether something was changed made incorrect assumptions.

Specifically, `dedent` will rejoin the lines using LF. When compared to the source which may have CRLF or just CR, this condition will never be true, even if nothing visually actually changed.

An alternative solution would be to reimplement dedent and indent to be aware of the line endings. Something we eventually might have to do, as there are still some places that (incorrectly) assume line endings are always just LF.

Closes #1490

---

When applied to #1501 this fixes the test failures.

---

_Comment by @charliermarsh on 2022-12-31 12:35_

Iiiinteresting, thank you, great find.

---

_Merged by @charliermarsh on 2022-12-31 12:37_

---

_Closed by @charliermarsh on 2022-12-31 12:37_

---

_Branch deleted on 2022-12-31 12:43_

---
