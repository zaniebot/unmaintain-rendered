```yaml
number: 860
title: "Fix clippy::inefficient-to-string (pedantic)"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-inefficient-to-string
created_at: 2022-11-21T19:21:24Z
updated_at: 2022-11-21T20:59:36Z
url: https://github.com/astral-sh/ruff/pull/860
synced_at: 2026-01-12T05:48:46Z
```

# Fix clippy::inefficient-to-string (pedantic)

---

_Pull request opened by @andersk on 2022-11-21 19:21_

“`&str` implements `ToString` through a slower blanket impl, but `str` has a fast specialization of `ToString`”

https://rust-lang.github.io/rust-clippy/master/index.html#inefficient_to_string

---

_Merged by @charliermarsh on 2022-11-21 20:59_

---

_Closed by @charliermarsh on 2022-11-21 20:59_

---
