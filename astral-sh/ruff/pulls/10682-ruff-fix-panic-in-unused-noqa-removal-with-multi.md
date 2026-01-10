```yaml
number: 10682
title: "[`ruff`] Fix panic in unused `# noqa` removal with multi-byte space (`RUF100`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: fix-ruf100-panic
created_at: 2024-03-31T19:21:18Z
updated_at: 2024-04-01T01:05:54Z
url: https://github.com/astral-sh/ruff/pull/10682
synced_at: 2026-01-10T22:47:02Z
```

# [`ruff`] Fix panic in unused `# noqa` removal with multi-byte space (`RUF100`)

---

_Pull request opened by @diceroll123 on 2024-03-31 19:21_

## Summary

Currently, [this line](https://github.com/astral-sh/ruff/blob/716688d44ecda5d4648b7a31556bb77afe6cbbc6/crates/ruff_linter/src/fix/edits.rs#L101) assumes that the `noqa` comment begins with an octothorpe followed by a space. (`# `) With anyone's random code, this of course is not always true.

When there's a multi-byte character after the leading octothorpe, such as [`\u0085`](https://www.fileformat.info/info/unicode/char/85/index.htm), we try slicing from within the character, causing a panic.

To fix this, the logic has been changed to remove unused `noqa` directives and keep any trailing comments, or removing the whole comment if the comment is just the unused `noqa`

Fixes #10097.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-03-31 19:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Fix panic in RUF100" to "[`ruff`] - Fix panic in (`RUF100`)" by @diceroll123 on 2024-03-31 19:46_

---

_Renamed from "[`ruff`] - Fix panic in (`RUF100`)" to "[`ruff`] - Fix panic (`RUF100`)" by @diceroll123 on 2024-03-31 19:47_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-01 00:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-01 00:42_

---

_Label `bug` added by @charliermarsh on 2024-04-01 00:42_

---

_@charliermarsh approved on 2024-04-01 00:52_

Thanks!

---

_Renamed from "[`ruff`] - Fix panic (`RUF100`)" to "[`ruff`] Fix panic in unused `# noqa` removal with multi-byte space (`RUF100`)" by @charliermarsh on 2024-04-01 00:52_

---

_Label `fuzzer` added by @charliermarsh on 2024-04-01 00:52_

---

_Merged by @charliermarsh on 2024-04-01 01:00_

---

_Closed by @charliermarsh on 2024-04-01 01:00_

---
