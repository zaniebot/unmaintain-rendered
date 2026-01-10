```yaml
number: 1962
title: "Don't write pip compile output to stdout with `-q`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/editable
created_at: 2024-02-25T02:57:13Z
updated_at: 2024-02-25T03:06:20Z
url: https://github.com/astral-sh/uv/pull/1962
synced_at: 2026-01-10T14:54:43Z
```

# Don't write pip compile output to stdout with `-q`

---

_Pull request opened by @charliermarsh on 2024-02-25 02:57_

## Summary

When the user provides an output file, avoid writing the `pip compile` output to `stdout` when `-q` is specified. (We still write to `stdout` if no output file is provided, since otherwise, the resolution won't be printed _anywhere_.)

---

_Label `cli` added by @charliermarsh on 2024-02-25 02:57_

---

_@charliermarsh reviewed on 2024-02-25 02:57_

---

_Review comment by @charliermarsh on `requirements.in`:2 on 2024-02-25 02:57_

(Accidentally included in a prior PR.)

---

_Merged by @charliermarsh on 2024-02-25 03:06_

---

_Closed by @charliermarsh on 2024-02-25 03:06_

---

_Branch deleted on 2024-02-25 03:06_

---
