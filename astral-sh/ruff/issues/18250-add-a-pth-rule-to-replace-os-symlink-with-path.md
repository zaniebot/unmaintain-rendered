```yaml
number: 18250
title: Add a PTH rule to replace os.symlink with Path.symlink_to
type: issue
state: closed
author: IlanCosman
labels:
  - rule
assignees: []
created_at: 2025-05-22T07:05:26Z
updated_at: 2025-05-28T10:39:07Z
url: https://github.com/astral-sh/ruff/issues/18250
synced_at: 2026-01-12T15:54:56Z
```

# Add a PTH rule to replace os.symlink with Path.symlink_to

---

_@IlanCosman_

### Summary

Just another place where someone might use `os` but can instead use pathlib. Would be nice to have it called out by the linter.

---

_Comment by @MichaReiser on 2025-05-22 08:42_

Reference: [`symlink_to`](https://docs.python.org/3/library/pathlib.html#pathlib.Path.symlink_to)

---

_Label `rule` added by @MichaReiser on 2025-05-22 08:42_

---

_Comment by @fennr on 2025-05-28 03:05_

@MichaReiser I would appreciate a review of my PR

---

_Closed by @MichaReiser on 2025-05-28 10:39_

---
