```yaml
number: 240
title: Handle filesystem errors more consistently
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: fs-errors
created_at: 2022-09-21T02:15:55Z
updated_at: 2022-09-21T04:15:45Z
url: https://github.com/astral-sh/ruff/pull/240
synced_at: 2026-01-12T15:55:04Z
```

# Handle filesystem errors more consistently

---

_@andersk_

Emit E902 for all filesystem errors that occur during directory walking or linting. Use the system-provided error message instead of assuming that all filesystem errors are “No such file or directory”. Remove the redundant `exists()` checks since we are no longer suppressing that error during directory walking.

---

_@charliermarsh reviewed on 2022-09-21 02:33_

---

_Review comment by @charliermarsh on `src/main.rs`:144 on 2022-09-21 02:33_

I think this was present specifically to handle the user explicitly providing a path to a file that doesn't exist. Are we still raising an error, similar to Flake8?

```
∴ flake8 foo.py
foo.py:0:1: E902 FileNotFoundError: [Errno 2] No such file or directory: 'foo.py'
```

---

_@andersk reviewed on 2022-09-21 03:06_

---

_Review comment by @andersk on `src/main.rs`:144 on 2022-09-21 03:06_

Yep.

```
$ ruff foo.py
Found 1 error(s).
foo.py:0:0: E902 No such file or directory (os error 2)
```

---

_Comment by @charliermarsh on 2022-09-21 03:21_

Awesome, thanks!

---

_Merged by @charliermarsh on 2022-09-21 03:22_

---

_Closed by @charliermarsh on 2022-09-21 03:22_

---

_Branch deleted on 2022-09-21 04:15_

---
