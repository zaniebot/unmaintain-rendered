```yaml
number: 5755
title: Fix the default value of python-preference in docs/reference/settings.md
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
assignees: []
merged: true
base: main
head: FishAlchemist-docs-patch
created_at: 2024-08-03T18:50:07Z
updated_at: 2024-08-10T08:28:23Z
url: https://github.com/astral-sh/uv/pull/5755
synced_at: 2026-01-12T16:06:59Z
```

# Fix the default value of python-preference in docs/reference/settings.md

---

_@FishAlchemist_

## Summary
After referring to https://github.com/astral-sh/uv/pull/5637 and doing additional testing.
The default value in a stable state seems more reasonable to be ``only-system``.  ``managed`` in preview.
```
cpython-3.11.9-windows-x86_64-none     C:\Users\name\AppData\Local\Programs\Python\Python311\python.exe

cpython-3.10.14-windows-x86_64-none    C:\Users\name\AppData\Roaming\uv\data\python\cpython-3.10.14-windows-x86_64-none\install\python.exe
cpython-3.10.11-windows-x86_64-none    C:\Users\name\AppData\Local\Programs\Python\Python310\python.exe

cpython-3.9.19-windows-x86_64-none     C:\Users\name\AppData\Roaming\uv\data\python\cpython-3.9.19-windows-x86_64-none\python.exe
```
test on uv  0.2.33  (build from https://github.com/astral-sh/uv/tree/257007ccaf0912a41dfe91b534527a7b1f54e19a)
### Stable version
``uv venv -p 3.10`` is ``3.10.11`` (System Python)
``uv venv -p 3.9`` is ``No interpreter found``(3.9.19 for managed Python)
``uv venv -p 3.9 --python-preference only-system`` is ``No interpreter found``(fail)
``uv venv -p 3.9 --python-preference only-managed`` is ``3.9.19``(success)
Do not use managed Python, only use the system Python, so it can be determined as ``only-system``.
### Preview mode
**Note:**  ``3.10.14`` is managed python, ``3.10.11`` is system python.
``uv venv -p 3.11 --preview`` is ``3.11.9`` (System Python)
``uv venv -p 3.10 --preview`` is ``3.10.14``
``uv venv -p 3.10 --preview --python-preference only-managed`` is ``3.10.14``
``uv venv -p 3.10 --preview --python-preference managed`` is ``3.10.14``
``uv venv -p 3.10 --preview --python-preference system`` is ``3.10.11``
``venv -p 3.10 --preview --python-preference only-system`` is ``3.10.11``
Prioritize the managed Python and then select the system Python, so it can be determined as ``managed``.

-----
fixed #5754 
## Test Plan
Run website in local.
![image](https://github.com/user-attachments/assets/02b81d44-1a99-4165-aca5-6ce46345b539)



---

_Comment by @zanieb on 2024-08-03 21:39_

Hey! This file is generated. I think you'd need to fix it in the definition itself?

---

_@charliermarsh approved on 2024-08-03 23:28_

I gotcha. Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-08-03 23:28_

---

_Merged by @charliermarsh on 2024-08-03 23:37_

---

_Closed by @charliermarsh on 2024-08-03 23:37_

---

_Branch deleted on 2024-08-10 08:28_

---
