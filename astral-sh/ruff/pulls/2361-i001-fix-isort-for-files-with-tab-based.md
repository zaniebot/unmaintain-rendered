```yaml
number: 2361
title: "[`I001`] fix isort for files with tab-based indentation"
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: isort-creates-mixed-indentation
created_at: 2023-01-30T20:28:05Z
updated_at: 2023-01-30T20:46:10Z
url: https://github.com/astral-sh/ruff/pull/2361
synced_at: 2026-01-12T15:55:08Z
```

# [`I001`] fix isort for files with tab-based indentation

---

_@sciyoshi_

This PR fixes two related issues with using isort on files using tabs for indentation:

- Multiline imports are never considered correctly formatted, since the comparison with the generated code will always fail.
- Using autofix generates code that can have mixed indentation in the same line, for imports that are within nested blocks.

---

_Comment by @charliermarsh on 2023-01-30 20:30_

Oh wow, totally missed this... Thank you!

---

_Merged by @charliermarsh on 2023-01-30 20:36_

---

_Closed by @charliermarsh on 2023-01-30 20:36_

---

_Branch deleted on 2023-01-30 20:46_

---
