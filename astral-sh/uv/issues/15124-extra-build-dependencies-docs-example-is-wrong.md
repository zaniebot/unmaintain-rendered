```yaml
number: 15124
title: "`extra-build-dependencies` docs example is wrong"
type: issue
state: closed
author: konstin
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2025-08-07T08:01:22Z
updated_at: 2025-09-02T13:06:22Z
url: https://github.com/astral-sh/uv/issues/15124
synced_at: 2026-01-12T16:02:04Z
```

# `extra-build-dependencies` docs example is wrong

---

_@konstin_

The example rendering for `extra-build-dependencies` is wrong at https://docs.astral.sh/uv/reference/settings/#extra-build-dependencies:

<img width="1063" height="670" alt="Image" src="https://github.com/user-attachments/assets/c932c14f-4752-4a48-b90f-21c7c7e691ec" />

---

_Label `documentation` added by @konstin on 2025-08-07 08:01_

---

_Comment by @zanieb on 2025-08-08 00:56_

What.. I thought I fixed this. I guess not though, thanks!

---

_Label `good first issue` added by @zanieb on 2025-08-08 00:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-08 09:37_

---

_Closed by @charliermarsh on 2025-08-11 21:24_

---

_Reopened by @zanieb on 2025-08-12 02:12_

---

_Closed by @charliermarsh on 2025-08-16 09:43_

---

_Reopened by @konstin on 2025-08-18 15:47_

---

_Comment by @zanieb on 2025-08-18 15:51_

@konstin what's up? This was fixed in a subsequent commit.

---

_Comment by @konstin on 2025-08-18 15:56_

It doesn't seems partially fixed, but not in all locations:

https://docs.astral.sh/uv/reference/settings/#pip_extra-build-dependencies

<img width="1058" height="649" alt="Image" src="https://github.com/user-attachments/assets/94ce821c-8d8d-483a-ad70-370138328395" />

https://docs.astral.sh/uv/reference/settings/#extra-build-dependencies

<img width="1058" height="649" alt="Image" src="https://github.com/user-attachments/assets/2d357e52-9616-4446-8f88-76e171a783df" />

I'm not fully following with the revert, but I can see those two cases on main.

---

_Closed by @charliermarsh on 2025-09-02 13:06_

---
