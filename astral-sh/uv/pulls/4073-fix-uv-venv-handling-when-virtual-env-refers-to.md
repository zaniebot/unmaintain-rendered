```yaml
number: 4073
title: "Fix `uv venv` handling when `VIRTUAL_ENV` refers to an non-existant environment"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-venv
created_at: 2024-06-05T20:44:32Z
updated_at: 2024-06-05T20:50:30Z
url: https://github.com/astral-sh/uv/pull/4073
synced_at: 2026-01-12T16:06:01Z
```

# Fix `uv venv` handling when `VIRTUAL_ENV` refers to an non-existant environment

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4072

This was an accidental change in https://github.com/astral-sh/uv/pull/4029 in which I had updated the pull request to support virtual environments as requested in review and forgot to revert it.

Separately, we shouldn't fail if `VIRTUAL_ENV` points to an empty directory and `SystemPython::Allowed` is used, will address that separately.

---

_Label `bug` added by @zanieb on 2024-06-05 20:44_

---

_Comment by @zanieb on 2024-06-05 20:46_

```
❯  VIRTUAL_ENV=/tmp/venv uv venv
  × failed to canonicalize path `/tmp/venv/bin/python3`
  ╰─▶ No such file or directory (os error 2)

❯ VIRTUAL_ENV=/tmp/venv cargo run -- venv -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/uv venv -v`
DEBUG Searching for Python interpreter in search path
DEBUG Found CPython 3.9.6 at `/usr/bin/python3` (search path)
Using Python 3.9.6 interpreter at: /Library/Developer/CommandLineTools/usr/bin/python3
Creating virtualenv at: .venv
INFO Removing existing directory
Activate with: source .venv/bin/activate
```

---

_@charliermarsh approved on 2024-06-05 20:47_

---

_Merged by @zanieb on 2024-06-05 20:50_

---

_Closed by @zanieb on 2024-06-05 20:50_

---

_Branch deleted on 2024-06-05 20:50_

---
