---
number: 3502
title: C4 should not be remapped to C40, leaves out C41
type: issue
state: closed
author: onerandomusername
labels: []
assignees: []
created_at: 2023-03-14T05:05:43Z
updated_at: 2023-03-15T16:58:07Z
url: https://github.com/astral-sh/ruff/issues/3502
synced_at: 2026-01-07T13:12:14-06:00
---

# C4 should not be remapped to C40, leaves out C41

---

_Issue opened by @onerandomusername on 2023-03-14 05:05_

Newest ruff version remaps C4 to C40 but that leaves out C41 of flake8-comphrehensions. https://beta.ruff.rs/docs/rules/#flake8-comprehensions-c4

version: ruff 0.0.255


---

_Comment by @calumy on 2023-03-14 07:20_

I think this might be a duplicate of #3486, which was fixed by #3488.

---

_Comment by @charliermarsh on 2023-03-14 14:23_

Yup! This has been fixed.

---

_Closed by @charliermarsh on 2023-03-14 14:23_

---

_Comment by @onerandomusername on 2023-03-15 16:46_

Whoops, sorry! I searched for duplicates but hadn't found it, thanks! 

---

_Comment by @charliermarsh on 2023-03-15 16:58_

No prob :)

---
