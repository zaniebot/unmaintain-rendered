```yaml
number: 15026
title: Gracefully handle entrypoint permission errors
type: pull_request
state: merged
author: louiswpf
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-run-permission-error
created_at: 2025-08-02T06:59:51Z
updated_at: 2025-08-02T12:03:37Z
url: https://github.com/astral-sh/uv/pull/15026
synced_at: 2026-01-10T06:53:02Z
```

# Gracefully handle entrypoint permission errors

---

_Pull request opened by @louiswpf on 2025-08-02 06:59_

Gracefully handle entrypoint permission errors

`uv run --with` could fail with a "permission denied" error when it
tried to copy an entrypoint with restrictive permissions.

For instance:

```sh
$ stat -c '%A' /usr/bin/groupmems
-rwxr-s---

$ uv python find
/usr/bin/python

$ uv run --with dummy_test
error: failed to open file `/usr/bin/groupmems`: Permission denied (os error 13)
```

The entrypoint copying logic now catches these permission errors and
skips the file, making `uv` more resilient on systems with binaries that
have restrictive permissions.

---

_@zanieb approved on 2025-08-02 12:03_

Thanks!

---

_Label `bug` added by @zanieb on 2025-08-02 12:03_

---

_Merged by @zanieb on 2025-08-02 12:03_

---

_Closed by @zanieb on 2025-08-02 12:03_

---
