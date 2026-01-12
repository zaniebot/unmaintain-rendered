```yaml
number: 482
title: Implement W605 (invalid escape sequence)
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/W605
created_at: 2022-10-26T23:09:21Z
updated_at: 2022-10-26T23:10:24Z
url: https://github.com/astral-sh/ruff/pull/482
synced_at: 2026-01-12T15:55:04Z
```

# Implement W605 (invalid escape sequence)

---

_@charliermarsh_

This PR implements W605 (the "invalid escape sequence" check). To do so, I've introduced a new check runner, `check_tokens.rs`, which iterates over the token stream, similar to `pycodestyle`.

I've disabled these pycodestyle warnings by default for now, since I want to do some performance optimizations around the number of traversals we're doing for the token stream and source code.

Resolves #474.


---

_Merged by @charliermarsh on 2022-10-26 23:10_

---

_Closed by @charliermarsh on 2022-10-26 23:10_

---

_Branch deleted on 2022-10-26 23:10_

---
