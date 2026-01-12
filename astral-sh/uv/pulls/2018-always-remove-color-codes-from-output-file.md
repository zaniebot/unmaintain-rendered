```yaml
number: 2018
title: Always remove color codes from output file
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/always-strip-files
created_at: 2024-02-27T15:45:17Z
updated_at: 2024-02-28T11:42:32Z
url: https://github.com/astral-sh/uv/pull/2018
synced_at: 2026-01-12T16:04:50Z
```

# Always remove color codes from output file

---

_@konstin_

Always strip color codes when we're writing to a file.

I don't really know how to test this.

Fixes #2017


---

_Label `bug` added by @konstin on 2024-02-27 15:45_

---

_@charliermarsh approved on 2024-02-27 15:57_

---

_Comment by @charliermarsh on 2024-02-27 15:58_

I don't understand why the autostream doesn't work here.

---

_@zanieb approved on 2024-02-27 15:58_

---

_Merged by @konstin on 2024-02-27 16:02_

---

_Closed by @konstin on 2024-02-27 16:02_

---

_Branch deleted on 2024-02-27 16:02_

---

_Comment by @konstin on 2024-02-27 16:10_

It seems that we eventually call `libc::isatty` with the file descriptor, but i don't know why the file descriptor is suddenly a tty in jupyter.

---

_Comment by @bluss on 2024-02-27 17:18_

It is very mysterious, but it seems like this might be the likely explanation: Something in ipython sets CLICOLOR_FORCE=1 and autostream listens to this env variable as well as a few others.

Maybe this is the culprit: https://github.com/ipython/ipykernel/blob/c1d944ece0eaeb5b1ea030f22a0e259d8fd915fd/ipykernel/zmqshell.py#L503-L504

confirmed by workaround `!env -u CLICOLOR_FORCE rye sync`

also related, maybe https://github.com/rust-cli/anstyle/issues/178

---
