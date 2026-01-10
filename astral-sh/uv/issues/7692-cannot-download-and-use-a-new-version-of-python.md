---
number: 7692
title: Cannot download and use a new version of python
type: issue
state: closed
author: knaus
labels: []
assignees: []
created_at: 2024-09-25T19:35:37Z
updated_at: 2024-09-25T19:37:44Z
url: https://github.com/astral-sh/uv/issues/7692
synced_at: 2026-01-10T01:24:18Z
---

# Cannot download and use a new version of python

---

_Issue opened by @knaus on 2024-09-25 19:35_

* A minimal code snippet that reproduces the bug.
<img width="518" alt="image" src="https://github.com/user-attachments/assets/83111dbf-0b97-44f4-aee5-94c279c23601">

```
uv venv cguvenv --python 3.11
uv python install 3.11
uv venv cguvenv --python 3.11
```

* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
<img width="597" alt="image" src="https://github.com/user-attachments/assets/232aa1d1-e42c-4bb1-869d-126dfa011a3f">

* The current uv platform. 
Terminal on MacBook Pro M3Pro
* The current uv version (`uv --version`).
<img width="446" alt="image" src="https://github.com/user-attachments/assets/b5216f8f-66a7-40b2-95e9-b6c4e0e5ba73">



---

_Comment by @knaus on 2024-09-25 19:37_

Fixed by updating UV to `v0.4.16`

---

_Closed by @knaus on 2024-09-25 19:37_

---
