---
number: 5064
title: "Add `uv lock` without resolution benchmark to codspeed "
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-07-15T08:21:54Z
updated_at: 2024-08-20T18:22:17Z
url: https://github.com/astral-sh/uv/issues/5064
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `uv lock` without resolution benchmark to codspeed 

---

_Issue opened by @konstin on 2024-07-15 08:21_

In #4860, we found `uv lock` took a long time before and after the resolution. We should add a codspeed benchmark for a large `uv lock` case with the resolution stubbed. This will give us automated and reproducible feedback on e.g. toml parser changes.

---

_Label `performance` added by @konstin on 2024-07-15 08:21_

---

_Label `preview` added by @konstin on 2024-07-15 08:21_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---
