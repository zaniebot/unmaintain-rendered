```yaml
number: 6274
title: "Use atomic write for `pip compile` output"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/safe
created_at: 2024-08-20T20:30:18Z
updated_at: 2024-08-22T00:08:32Z
url: https://github.com/astral-sh/uv/pull/6274
synced_at: 2026-01-12T16:07:18Z
```

# Use atomic write for `pip compile` output

---

_@charliermarsh_

## Summary

This ensures that we don't stream output to the `--output-file`, since other processes may rely on reading it.

Closes https://github.com/astral-sh/uv/issues/6239.


---

_Label `bug` added by @charliermarsh on 2024-08-20 20:30_

---

_@zanieb approved on 2024-08-20 20:31_

---

_Merged by @charliermarsh on 2024-08-20 20:36_

---

_Closed by @charliermarsh on 2024-08-20 20:36_

---

_Branch deleted on 2024-08-20 20:36_

---

_Comment by @notatallshaw on 2024-08-22 00:07_

This burns everyone, that need to open a new Python process on Windows, eventually, there's in fact a big red warning about it in the docs: https://docs.python.org/3/library/subprocess.html#popen-constructor

---
