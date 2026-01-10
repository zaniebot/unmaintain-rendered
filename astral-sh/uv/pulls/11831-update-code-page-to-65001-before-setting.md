```yaml
number: 11831
title: "Update code page to 65001 before setting environment variables, fix #11828"
type: pull_request
state: merged
author: flaribbit
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-27T11:33:34Z
updated_at: 2025-03-03T05:19:42Z
url: https://github.com/astral-sh/uv/pull/11831
synced_at: 2026-01-10T11:10:39Z
```

# Update code page to 65001 before setting environment variables, fix #11828

---

_Pull request opened by @flaribbit on 2025-02-27 11:33_

## Summary

When executing `.venv\Scripts\activate` in `cmd`, the script uses the local codepage for execution. This causes issues when the file path contains non-ASCII characters, resulting in corrupted environment variables such as `VIRTUAL_ENV`.

See #11828.

Code used to fix the issue was adapted from https://github.com/python/cpython/blob/3.13/Lib/venv/scripts/nt/activate.bat#L3-L9.

## Test Plan

Before:
![image](https://github.com/user-attachments/assets/f8a50675-a688-4b4b-9d1b-0a5d5f736123)

After:
![image](https://github.com/user-attachments/assets/3aed978e-4c9b-40e4-a689-9a602f95b725)
(`chcp` command cleared the history lol)

---

_Comment by @charliermarsh on 2025-02-27 14:59_

Thanks! Do you know why this isn't present already? Could you check the history of [virtualenv](https://github.com/pypa/virtualenv) to see why they're not in there?

---

_Label `windows` added by @charliermarsh on 2025-02-27 14:59_

---

_Comment by @flaribbit on 2025-02-27 15:07_

Ok. I found that virtualenv fixed the issue in https://github.com/pypa/virtualenv/pull/2687

---

_@charliermarsh approved on 2025-02-27 21:14_

---

_Merged by @charliermarsh on 2025-02-27 21:14_

---

_Closed by @charliermarsh on 2025-02-27 21:14_

---

_Label `bug` added by @charliermarsh on 2025-02-27 21:14_

---

_Comment by @charliermarsh on 2025-02-27 21:15_

Thanks! It looks like it was added after we vendored these files (but already existed upstream in CPython).

---

_Branch deleted on 2025-03-03 05:19_

---
