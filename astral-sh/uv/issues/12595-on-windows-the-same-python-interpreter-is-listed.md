```yaml
number: 12595
title: On Windows, the same Python interpreter is listed twice with different folder name case
type: issue
state: closed
author: ArneBachmannDLR
labels:
  - bug
assignees: []
created_at: 2025-04-01T06:36:39Z
updated_at: 2025-04-07T18:59:00Z
url: https://github.com/astral-sh/uv/issues/12595
synced_at: 2026-01-12T16:01:07Z
```

# On Windows, the same Python interpreter is listed twice with different folder name case

---

_@ArneBachmannDLR_

### Summary

![Image](https://github.com/user-attachments/assets/feffe14a-f916-4f09-bba4-67755db6757f)

I cannot retrace what actions let to this situation, but there should probably be a normalization step that prevents this.

### Platform

Windows 11

### Version

23H2

### Python version

Python 3.13.2 (vs. Mambaforge (Miniforge conda) 24.3)

---

_Label `bug` added by @ArneBachmannDLR on 2025-04-01 06:36_

---

_Comment by @PhiFever on 2025-04-01 07:41_

Familiar problem, on win11 I use scoop to manage my python environment and python 3.12 has been listed twice

![Image](https://github.com/user-attachments/assets/2c2133c5-01a9-4980-982b-69ce2dee690a)

---

_Comment by @charliermarsh on 2025-04-01 12:59_

I think this would be closed by #12367.

---

_Assigned to @zanieb by @charliermarsh on 2025-04-01 12:59_

---

_Closed by @Gankra on 2025-04-07 18:59_

---
