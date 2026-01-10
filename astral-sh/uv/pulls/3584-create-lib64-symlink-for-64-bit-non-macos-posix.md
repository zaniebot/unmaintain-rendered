```yaml
number: 3584
title: Create lib64 symlink for 64-bit, non-macOS, POSIX environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/syslink
created_at: 2024-05-14T17:47:14Z
updated_at: 2024-05-14T18:33:45Z
url: https://github.com/astral-sh/uv/pull/3584
synced_at: 2026-01-10T14:37:54Z
```

# Create lib64 symlink for 64-bit, non-macOS, POSIX environments

---

_Pull request opened by @charliermarsh on 2024-05-14 17:47_

## Summary

Closes https://github.com/astral-sh/uv/issues/3578#issuecomment-2110675382.

## Test Plan

Verified that in the OpenSUSE test, we create both, and they're symlinks:

```text
INFO: Creating virtual environment with `venv`...
INFO: Installing into `venv` virtual environment...
DEBUG Found a virtualenv named .venv at: /tmp/tmp4nape29h/.venv
DEBUG Cached interpreter info for Python 3.10.14, skipping probing: .venv/bin/python
DEBUG Using Python 3.10.14 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
purelib: "/tmp/tmp4nape29h/.venv/lib/python3.10/site-packages"
platlib: "/tmp/tmp4nape29h/.venv/lib64/python3.10/site-packages"
is_same_file(purelib, platlib): Ok(true)
```


---

_Review requested from @konstin by @charliermarsh on 2024-05-14 17:47_

---

_Marked ready for review by @charliermarsh on 2024-05-14 17:47_

---

_Label `bug` added by @charliermarsh on 2024-05-14 17:47_

---

_Label `compatibility` added by @charliermarsh on 2024-05-14 17:47_

---

_Comment by @konstin on 2024-05-14 18:04_

Code lgtm but it's odd because `virtualenv` doesn't create this directory for me

---

_Comment by @charliermarsh on 2024-05-14 18:07_

Would you vote against merging? I tend to view `venv` as the reference implementation but also agree it's strange.

---

_Comment by @konstin on 2024-05-14 18:12_

The extra directory wont hurt, but it would be good to understand what `virtualenv` does here 

---

_Comment by @charliermarsh on 2024-05-14 18:15_

@gaborbernat - sorry to bother, have you seen this before?

---

_Comment by @gaborbernat on 2024-05-14 18:33_

Could be bug, I know Fedora creates two folders and should work.

---

_Merged by @charliermarsh on 2024-05-14 18:33_

---

_Closed by @charliermarsh on 2024-05-14 18:33_

---

_Branch deleted on 2024-05-14 18:33_

---
