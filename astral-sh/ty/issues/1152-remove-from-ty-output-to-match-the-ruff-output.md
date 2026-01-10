```yaml
number: 1152
title: "Remove \" --> \" from ty output to match the ruff output format and make file paths clickable in IntelliJ IDEA products"
type: issue
state: closed
author: qarmin
labels: []
assignees: []
created_at: 2025-09-08T09:21:49Z
updated_at: 2025-09-08T09:26:21Z
url: https://github.com/astral-sh/ty/issues/1152
synced_at: 2026-01-10T02:06:25Z
```

# Remove " --> " from ty output to match the ruff output format and make file paths clickable in IntelliJ IDEA products

---

_Issue opened by @qarmin on 2025-09-08 09:21_

### Summary

Ty default output(`ty check .`):
<img width="1016" height="80" alt="Image" src="https://github.com/user-attachments/assets/815ade7e-19a5-4816-accb-7a825cefbb7d" />

Ruff default output(`ruff check .`):
<img width="1234" height="102" alt="Image" src="https://github.com/user-attachments/assets/a4aecb2d-952e-435a-a64a-58a3d4182214" />

Ty after modification(`ty check . | sed 's/^  --> //'`) - loses colors
<img width="1234" height="102" alt="Image" src="https://github.com/user-attachments/assets/dc8f17d2-78a3-43a2-98d3-7f954764ab24" />

### Version

Alpha 16

---

_Comment by @sharkdp on 2025-09-08 09:26_

Thank you. This looks like a duplicate of https://github.com/astral-sh/ty/issues/1079.

---

_Closed by @sharkdp on 2025-09-08 09:26_

---
