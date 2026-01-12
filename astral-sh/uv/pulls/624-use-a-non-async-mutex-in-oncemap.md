```yaml
number: 624
title: "Use a non-async `Mutex` in `OnceMap`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/await
created_at: 2023-12-12T19:46:08Z
updated_at: 2023-12-12T19:59:46Z
url: https://github.com/astral-sh/uv/pull/624
synced_at: 2026-01-12T16:04:04Z
```

# Use a non-async `Mutex` in `OnceMap`

---

_@charliermarsh_

I don't know why, but this seems to resolve https://github.com/astral-sh/puffin/issues/619. The Tokio docs also say that using Tokio's Mutex is _not_ recommended unless you need to hold the Mutex across an `.await`, which we don't.

Since this is a non-deterministic failure, I just ran it a bunch of times and ensured it didn't hang (whereas it did hang occasionally prior to this PR).

Closes https://github.com/astral-sh/puffin/issues/619


---

_Label `bug` added by @charliermarsh on 2023-12-12 19:46_

---

_Merged by @charliermarsh on 2023-12-12 19:59_

---

_Closed by @charliermarsh on 2023-12-12 19:59_

---

_Branch deleted on 2023-12-12 19:59_

---
