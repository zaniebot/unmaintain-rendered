```yaml
number: 1036
title: Extend and rename RUF004 to PLR1722
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: rename-and-extend-ruf004-plr1722
created_at: 2022-12-04T15:08:34Z
updated_at: 2023-04-30T19:47:28Z
url: https://github.com/astral-sh/ruff/pull/1036
synced_at: 2026-01-12T15:55:05Z
```

# Extend and rename RUF004 to PLR1722

---

_@JonathanPlasse_

- [x] Rename `RUF004` to `PLR1722`
- [x] Consider using `sys.exit()` for `quit()`
- [x] Move the rules from pylint plugins to their individual file.

---

_Comment by @charliermarsh on 2022-12-04 15:15_

Looks good, thanks!

---

_Comment by @charliermarsh on 2022-12-04 15:20_

In the future, it'd be preferable (e.g.) to break this into two PRs: one to split up `plugins.rs`, and another to do the `RUF004` to `PLR1722` translation. That makes it easier to understand the changes, and to revert if needed. But not necessary to redo here.


---

_Merged by @charliermarsh on 2022-12-04 15:20_

---

_Closed by @charliermarsh on 2022-12-04 15:20_

---

_Branch deleted on 2023-04-30 19:47_

---
