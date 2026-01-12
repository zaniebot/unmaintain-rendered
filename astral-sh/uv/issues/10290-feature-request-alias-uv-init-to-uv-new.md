```yaml
number: 10290
title: "[Feature Request] alias `uv init` to `uv new`"
type: issue
state: open
author: Heus-Sueh
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2025-01-03T15:37:00Z
updated_at: 2025-05-20T01:50:07Z
url: https://github.com/astral-sh/uv/issues/10290
synced_at: 2026-01-12T16:00:10Z
```

# [Feature Request] alias `uv init` to `uv new`

---

_@Heus-Sueh_

the word `new` is used in `cargo` and `poetry` to create new projects, I think it would make sense to have the word `new` as an alias for `uv init`, saving the need to implement a lot of things in the code and those who are used to `init` are not affected and those who are coming from `poetry` or `cargo` do not need to learn a new keyword.

the problem would be `uv new` without arguments initializing a project in the current directory

---

_Comment by @dsantiago on 2025-01-05 05:48_

I like the `init` term...

---

_Label `needs-decision` added by @charliermarsh on 2025-01-05 20:03_

---

_Label `cli` added by @charliermarsh on 2025-01-05 20:03_

---

_Comment by @BoltonBailey on 2025-05-19 22:28_

I think in both the case of `poetry` and `cargo`, the `new` command creates the project in a new directory, whereas `init` creates it in the current directory, so it would be nice if the commands for `uv` reflected that.

---

_Comment by @zanieb on 2025-05-20 01:50_

I don't think we should alias this since we might want the `new` command to do something else in the future (as described by @BoltonBailey). A special-cased error suggesting to use `uv init` seems nice and fine in the meantime though.

---
