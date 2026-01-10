---
number: 9813
title: "Add '--constraints'  flag to 'uv tool run'"
type: issue
state: closed
author: wpk-nist-gov
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-12-11T14:58:08Z
updated_at: 2025-03-04T02:18:49Z
url: https://github.com/astral-sh/uv/issues/9813
synced_at: 2026-01-10T01:24:46Z
---

# Add '--constraints'  flag to 'uv tool run'

---

_Issue opened by @wpk-nist-gov on 2024-12-11 14:58_

The `--constraints` flag was recently added to `uv tool install` via #9547.  It would be really useful to add a similar flag to `uv tool run`. 
This would allow specifying version constraints for tooling in one place.  For example, 

```python
uvx --constraints tool-constraints.txt ruff
uvx --constraints tool-constraints.txt codespell
...
```

Thank you for this amazing package!


---

_Label `enhancement` added by @zanieb on 2024-12-11 15:38_

---

_Label `help wanted` added by @charliermarsh on 2024-12-15 23:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 19:41_

---

_Referenced in [astral-sh/uv#10207](../../astral-sh/uv/pulls/10207.md) on 2024-12-27 19:42_

---

_Closed by @charliermarsh on 2025-03-04 02:18_

---

_Closed by @charliermarsh on 2025-03-04 02:18_

---
