---
number: 7116
title: "In Windows, `uv add` _sometimes_ adds extra lines between dependencies"
type: issue
state: closed
author: janpipek
labels: []
assignees: []
created_at: 2024-09-06T05:26:27Z
updated_at: 2024-09-06T06:45:35Z
url: https://github.com/astral-sh/uv/issues/7116
synced_at: 2026-01-10T01:24:10Z
---

# In Windows, `uv add` _sometimes_ adds extra lines between dependencies

---

_Issue opened by @janpipek on 2024-09-06 05:26_

When I add a dependency in Windows 10:

`uv add polars`

I get extra lines in the `pyproject.toml` file:

![image](https://github.com/user-attachments/assets/12bfbea3-b4e4-457a-a42a-f826753c8166)

I guess it has something to do with line endings on different OSes but you will know better, for sure.

```
uv version
uv 0.4.6 (84f25e8cf 2024-09-05)
```

Thanks for how smoothly this runs otherwise!

---

_Renamed from "In Windows, `uv add` adds extra lines between dependencies" to "In Windows, `uv add` _sometimes_ adds extra lines between dependencies" by @janpipek on 2024-09-06 06:30_

---

_Comment by @janpipek on 2024-09-06 06:30_

Eh, sorry. In another attempt, it did not do so :-(

---

_Closed by @janpipek on 2024-09-06 06:45_

---
