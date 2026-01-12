```yaml
number: 880
title: "Fix clippy::unnecessary_wraps (pedantic)"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fixes
created_at: 2022-11-22T23:25:20Z
updated_at: 2022-11-22T23:51:31Z
url: https://github.com/astral-sh/ruff/pull/880
synced_at: 2026-01-12T15:55:05Z
```

# Fix clippy::unnecessary_wraps (pedantic)

---

_@charliermarsh_

https://rust-lang.github.io/rust-clippy/master/index.html#unnecessary_wraps

---

_Merged by @charliermarsh on 2022-11-22 23:25_

---

_Closed by @charliermarsh on 2022-11-22 23:25_

---

_Branch deleted on 2022-11-22 23:25_

---

_Comment by @andersk on 2022-11-22 23:51_

There can be errors in `add_noqa` and `autoformat`; they’re just discarded by the `.flatten()` calls. We probably want to propagate or at least print them.

---
