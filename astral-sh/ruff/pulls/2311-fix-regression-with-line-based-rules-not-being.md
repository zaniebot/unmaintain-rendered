```yaml
number: 2311
title: Fix regression with line-based rules not being ignored per-file
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: line-based-per-file
created_at: 2023-01-28T19:24:55Z
updated_at: 2023-01-28T22:30:59Z
url: https://github.com/astral-sh/ruff/pull/2311
synced_at: 2026-01-12T15:55:07Z
```

# Fix regression with line-based rules not being ignored per-file

---

_@sciyoshi_

Apologies for the [broken previous PR](https://github.com/charliermarsh/ruff/pull/2309). I think there isn't much test coverage around per-file-ignores and I'm still getting used to the codebase!

---

_Comment by @sciyoshi on 2023-01-28 19:28_

I also haven't tested the interaction of having `RUF100` ignored per-file, which seems unlikely to be a very common case. `check_noqa` does mutate `diagnostics` however, so maybe the more correct but less efficient approach would be to pass `per_file_ignores` as an argument and have it checked again there.

---

_Comment by @charliermarsh on 2023-01-28 19:48_

No prob -- I think you're right that this placement is correct.

---

_Merged by @charliermarsh on 2023-01-28 19:48_

---

_Closed by @charliermarsh on 2023-01-28 19:48_

---

_Comment by @charliermarsh on 2023-01-28 19:48_

Appreciate all your contributions!

---

_Branch deleted on 2023-01-28 22:30_

---
