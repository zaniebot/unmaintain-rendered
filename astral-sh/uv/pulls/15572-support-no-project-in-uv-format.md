```yaml
number: 15572
title: "support `--no-project` in `uv format`"
type: pull_request
state: merged
author: younver
labels:
  - preview
assignees: []
merged: true
base: main
head: format-no-project
created_at: 2025-08-28T16:43:13Z
updated_at: 2025-09-09T08:09:35Z
url: https://github.com/astral-sh/uv/pull/15572
synced_at: 2026-01-12T16:11:49Z
```

# support `--no-project` in `uv format`

---

_@younver_

When a user passes `--no-project` argument to `uv format` command, instead of running the formatter in the context of the current project, run it in the context of the current directory. This is useful when the current directory is not a project.

Closes https://github.com/astral-sh/uv/issues/15462

---

_Label `preview` added by @zanieb on 2025-08-28 18:04_

---

_@zanieb reviewed on 2025-08-28 18:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-28 18:05_

I don't think these changes belong in this pull request, that will cause more changes in behavior than you're targeting here. See https://github.com/astral-sh/uv/pull/15553 

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:91 on 2025-08-28 21:42_

this also belongs to #15553

---

_@jorgehermo9 reviewed on 2025-08-28 21:42_

---

_@younver reviewed on 2025-08-28 22:11_

---

_Review comment by @younver on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-28 22:11_

You're right, removed all `WorkpaceError`s, kept the default `Err(err)` with `warn_user!`; thanks!

---

_@younver reviewed on 2025-08-28 22:21_

---

_Review comment by @younver on `crates/uv/src/commands/project/format.rs`:91 on 2025-08-28 22:21_

Totally missed that PR, so should I wait for your changes?

---

_@jorgehermo9 reviewed on 2025-08-31 21:10_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:91 on 2025-08-31 21:10_

I think it should be better to wait, I'm sorry I delayed that PR, had some personal issues and couldn't work on it

---

_@younver reviewed on 2025-09-01 23:28_

---

_Review comment by @younver on `crates/uv/src/commands/project/format.rs`:91 on 2025-09-01 23:28_

No worries, it happens. Feel free to give heads up whenever you're done with that PR.

---

_Comment by @younver on 2025-09-06 22:57_

Updated after #15553.

Can you review @zanieb & @jorgehermo9? Thanks.

---

_Comment by @jorgehermo9 on 2025-09-08 20:22_

Looks good from my side @younver ! Very nice PR

---

_@zanieb approved on 2025-09-08 21:16_

Thanks!

---

_Merged by @zanieb on 2025-09-08 21:16_

---

_Closed by @zanieb on 2025-09-08 21:16_

---

_Branch deleted on 2025-09-09 08:09_

---
