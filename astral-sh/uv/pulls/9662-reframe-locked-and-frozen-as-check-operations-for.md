```yaml
number: 9662
title: "Reframe `--locked` and `--frozen` as `--check` operations for `uv lock`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/lock-check
created_at: 2024-12-05T15:30:26Z
updated_at: 2024-12-08T16:02:02Z
url: https://github.com/astral-sh/uv/pull/9662
synced_at: 2026-01-12T16:08:55Z
```

# Reframe `--locked` and `--frozen` as `--check` operations for `uv lock`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7639

---

_Label `cli` added by @zanieb on 2024-12-05 15:30_

---

_Marked ready for review by @zanieb on 2024-12-05 16:50_

---

_Comment by @johnthagen on 2024-12-06 14:21_

@zanieb As a Poetry user, I think `uv lock --check` is a lot easier to understand as I was definitely confused by the meaning of `uv lock --locked` when trying to find the `uv` equivalent for `poetry check --lock`. Big 👍 from me.

---

_Review requested from @charliermarsh by @zanieb on 2024-12-06 14:23_

---

_Comment by @samypr100 on 2024-12-06 16:10_

> @zanieb As a Poetry user, I think `uv lock --check` is a lot easier to understand as I was definitely confused by the meaning of `uv lock --locked` when trying to find the `uv` equivalent for `poetry check --lock`. Big 👍 from me.

 @johnthagen this reminds me of https://github.com/python-poetry/poetry/pull/8015 😂

---

_Comment by @zanieb on 2024-12-06 20:13_

:D yeah. I think we're fairly far off from a `uv check` command but I would _presume_ it does something like this? ref #9653 

`uv check` might be expected to focus on your _code_ (given the amount of static analysis tooling we happen to have) so maybe that'd be a separate command? Multiple commands to check the project are definitely annoying though.

---

_@charliermarsh reviewed on 2024-12-08 15:02_

---

_Review comment by @charliermarsh on `docs/reference/cli.md`:1799 on 2024-12-08 15:02_

The env var / CLI name mismatch here is a bit strange, but oh well.

---

_@charliermarsh approved on 2024-12-08 15:02_

---

_Review comment by @zanieb on `docs/reference/cli.md`:1799 on 2024-12-08 16:01_

Yeah, I noticed that too but hopefully the "Equivalent to" call-out above is helpful. `UV_LOCK_CHECK` doesn't really make sense to set via the environment anyway.

---

_@zanieb reviewed on 2024-12-08 16:01_

---

_Merged by @zanieb on 2024-12-08 16:02_

---

_Closed by @zanieb on 2024-12-08 16:02_

---

_Branch deleted on 2024-12-08 16:02_

---
