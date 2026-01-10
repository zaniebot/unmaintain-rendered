```yaml
number: 6453
title: "Treat `.pyw` files as scripts in `uv run` on Windows"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/run-pyw
created_at: 2024-08-22T18:25:10Z
updated_at: 2024-08-22T23:07:32Z
url: https://github.com/astral-sh/uv/pull/6453
synced_at: 2026-01-10T13:09:51Z
```

# Treat `.pyw` files as scripts in `uv run` on Windows

---

_Pull request opened by @zanieb on 2024-08-22 18:25_

Closes https://github.com/astral-sh/uv/issues/6435

---

_Label `bug` added by @zanieb on 2024-08-22 18:25_

---

_Comment by @zanieb on 2024-08-22 18:25_

I haven't tested this. Do we need to use `pythonw` instead of `python` or something?

---

_Comment by @samypr100 on 2024-08-22 21:10_

> I haven't tested this. Do we need to use `pythonw` instead of `python` or something?

Yup

---

_Comment by @zanieb on 2024-08-22 21:17_

Tragic. Will do that then.

---

_Comment by @zanieb on 2024-08-22 21:20_

Is that right? it should be spawned in the background?

---

_Comment by @zanieb on 2024-08-22 21:27_

(Yes, I think it is right)

---

_Comment by @samypr100 on 2024-08-22 21:43_

I just tried your branch, seems to work.

---

_Comment by @zanieb on 2024-08-22 21:50_

Thank you!

---

_Review requested from @charliermarsh by @zanieb on 2024-08-22 21:50_

---

_@charliermarsh reviewed on 2024-08-22 22:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:838 on 2024-08-22 22:55_

Should the `||` here be removed? What's the difference between this and the next branch?

---

_@zanieb reviewed on 2024-08-22 22:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:838 on 2024-08-22 22:58_

Oh that's wrong, yes it should be removed.

---

_@zanieb reviewed on 2024-08-22 23:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:838 on 2024-08-22 23:00_

The next branch will use `pythonw.exe` to execute instead of `python.exe`

---

_@charliermarsh approved on 2024-08-22 23:02_

---

_Merged by @zanieb on 2024-08-22 23:07_

---

_Closed by @zanieb on 2024-08-22 23:07_

---

_Branch deleted on 2024-08-22 23:07_

---
