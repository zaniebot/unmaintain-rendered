```yaml
number: 20144
title: "[`flake8-use-pathlib`] Gate autofixes on extra args, preserve chmod `follow_symlinks` (`PTH101`, `PTH104`, `PTH105`, `PTH121`)"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
base: main
head: fix-20134
created_at: 2025-08-28T22:20:58Z
updated_at: 2025-09-21T23:48:34Z
url: https://github.com/astral-sh/ruff/pull/20144
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-use-pathlib`] Gate autofixes on extra args, preserve chmod `follow_symlinks` (`PTH101`, `PTH104`, `PTH105`, `PTH121`)

---

_Pull request opened by @danparizher on 2025-08-28 22:20_

## Summary

Fixes #20134


---

_Comment by @github-actions[bot] on 2025-08-28 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake_8_use_pathlib`] Gate autofixes on extra args, preserve chmod `follow_symlinks` (`PTH101`, `PTH104`, `PTH105`, `PTH121`)" to "[`flake8-use-pathlib`] Gate autofixes on extra args, preserve chmod `follow_symlinks` (`PTH101`, `PTH104`, `PTH105`, `PTH121`)" by @danparizher on 2025-08-29 00:20_

---

_Comment by @chirizxc on 2025-08-29 10:16_

It seems we made a PR for one issue

---

_Comment by @chirizxc on 2025-08-29 10:17_

I like your solution, but will autofix work if "False" is passed instead of False, in this case we also shouldn't offer fix

---

_Comment by @ntBre on 2025-08-29 13:23_

I was planning to review #20143 since it came in just before this one. Maybe we can add @danparizher as a co-author, especially if you like some of the code here and pull it into your PR.

---

_Closed by @danparizher on 2025-09-21 23:48_

---

_Branch deleted on 2025-09-21 23:48_

---
