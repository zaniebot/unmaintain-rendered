---
number: 8912
title: "Corrupted `uv.lock` File Prevents Update with `uv lock` and `uv sync`"
type: issue
state: closed
author: kiasari
labels:
  - question
assignees: []
created_at: 2024-11-08T00:54:20Z
updated_at: 2024-12-26T16:59:39Z
url: https://github.com/astral-sh/uv/issues/8912
synced_at: 2026-01-07T13:12:18-06:00
---

# Corrupted `uv.lock` File Prevents Update with `uv lock` and `uv sync`

---

_Issue opened by @kiasari on 2024-11-08 00:54_

uv 0.4.29
`uv lock` and `uv sync` can't update `uv.lock` file if a corrupted `uv.lock` exists.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-11-08 13:46_

What would you expect to happen here? (And do you know how the `uv.lock` was corrupted?)

---

_Label `question` added by @charliermarsh on 2024-11-08 13:46_

---

_Comment by @kiasari on 2024-11-08 17:24_

Similar to the process when no lock file exists, it should ideally generate a new `uv.lock` file to replace the corrupted one. The lock file is corrupted since both `uv lock` and `uv sync` return `error: Failed to parse uv.lock`. 

---

_Comment by @charliermarsh on 2024-11-09 13:52_

I think silently ignoring that lock like that seems a little dubious -- if it's due to a merge conflict, we'd basically throw away the merge conflict, which would be wrong. Cargo for example will error if you have a corrupt lockfile. It seems reasonable to me to expect the user to just `rm uv.lock`.

---

_Comment by @jgehrcke on 2024-11-09 14:18_

> It seems reasonable to me to expect the user to just rm uv.lock.

Agree. A "corrupt" lockfile is an exceptional event and I want to know when this happens.

---

_Comment by @kiasari on 2024-11-11 01:03_

After accidentally modifying the `uv.lock` file in Vim, I noticed that `uv sync` and `uv lock` stopped working and I had to delete the lock file.

---

_Closed by @charliermarsh on 2024-12-26 16:59_

---

_Comment by @charliermarsh on 2024-12-26 16:59_

Comfortable with our current behavior though acknowledge that there's subjectivity here.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-26 16:59_

---
