```yaml
number: 2301
title: Fix version shorthand detection to use -V instead of -v
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ac
created_at: 2023-01-28T15:43:05Z
updated_at: 2023-01-28T15:47:48Z
url: https://github.com/astral-sh/ruff/pull/2301
synced_at: 2026-01-12T15:55:07Z
```

# Fix version shorthand detection to use -V instead of -v

---

_@charliermarsh_

Fixes a regression introduced in https://github.com/charliermarsh/ruff/commit/eda2be635062c37dfaaccb630d2510cf458f8dd6 (but not yet released to users). (`-v` is a real flag, but it's an alias for `--verbose`, not `--version`.)

Closes #2299.

---

_Comment by @not-my-profile on 2023-01-28 15:44_

Mind prefixing `fix:` to clarify that this is a bug fix? Cause the commit message currently sounds like it's an UI change.

(You could also mention that this fixes a regression introduced in eda2be635062c37dfaaccb630d2510cf458f8dd6).

---

_Renamed from "Use -V instead of -v to detect version shorthand" to "Fix version shorthand detection to use -V instead of -v" by @charliermarsh on 2023-01-28 15:45_

---

_Merged by @charliermarsh on 2023-01-28 15:47_

---

_Closed by @charliermarsh on 2023-01-28 15:47_

---

_Branch deleted on 2023-01-28 15:47_

---
