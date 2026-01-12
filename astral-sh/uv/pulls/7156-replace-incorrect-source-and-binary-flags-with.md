```yaml
number: 7156
title: " Replace incorrect ``--source`` and ``--binary`` flags with correct ``--sdist`` and ``--wheel`` flags in ``uv build``"
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix_build_doc
created_at: 2024-09-07T03:37:49Z
updated_at: 2024-09-09T22:03:37Z
url: https://github.com/astral-sh/uv/pull/7156
synced_at: 2026-01-12T16:07:43Z
```

#  Replace incorrect ``--source`` and ``--binary`` flags with correct ``--sdist`` and ``--wheel`` flags in ``uv build``

---

_@FishAlchemist_


## Summary
 Replace incorrect ``--source`` and ``--binary`` flags with correct ``--sdist`` and ``--wheel`` flags in uv build.
 
![image](https://github.com/user-attachments/assets/f72a9c73-bfce-441b-8756-d0cb22afcd7d)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Run the server locally
![image](https://github.com/user-attachments/assets/f702f19f-96b8-4338-bd1b-e2fb35f5697a)

<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-09-07 11:50_

Thanks.

---

_Merged by @charliermarsh on 2024-09-07 11:50_

---

_Closed by @charliermarsh on 2024-09-07 11:50_

---

_Label `documentation` added by @charliermarsh on 2024-09-07 11:51_

---

_Branch deleted on 2024-09-07 12:02_

---

_Comment by @neutrinoceros on 2024-09-09 22:03_

This was listed in the Bugfixes section of uv 0.4.8's release notes. Shouldn't it be in the Documentation section instead ?

---
