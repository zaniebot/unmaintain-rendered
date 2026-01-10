---
number: 14453
title: tuple expansion resulting in missing args
type: issue
state: closed
author: Ryang20718
labels:
  - bug
assignees: []
created_at: 2025-07-04T01:49:44Z
updated_at: 2025-07-04T01:50:05Z
url: https://github.com/astral-sh/uv/issues/14453
synced_at: 2026-01-10T01:25:45Z
---

# tuple expansion resulting in missing args

---

_Issue opened by @Ryang20718 on 2025-07-04 01:49_

### Summary

 No arguments provided for required parameters `a`, `b` of bound method `randint`

repro
```
import random
ran= [1,4]
print("GG")
print(random.randint(*ran))
```

### Platform

ubuntu 22

### Version

ty==0.0.1a13

### Python version

3.10

---

_Label `bug` added by @Ryang20718 on 2025-07-04 01:49_

---

_Closed by @Ryang20718 on 2025-07-04 01:50_

---
