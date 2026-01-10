```yaml
number: 7891
title: "Check `python/uv/` folder with `mypy`"
type: pull_request
state: merged
author: sobolevn
labels:
  - internal
assignees: []
merged: true
base: main
head: check-python-uv-mypy
created_at: 2024-10-03T08:28:48Z
updated_at: 2024-10-03T12:05:07Z
url: https://github.com/astral-sh/uv/pull/7891
synced_at: 2026-01-10T12:53:58Z
```

# Check `python/uv/` folder with `mypy`

---

_Pull request opened by @sobolevn on 2024-10-03 08:28_

It used to report:

```
» mypy 
python/uv/_build_backend.py:40: error: Incompatible types in assignment (expression has type "list[str]", variable has type "CompletedProcess[bytes]")  [assignment]
python/uv/_build_backend.py:41: error: Value of type "CompletedProcess[bytes]" is not indexable  [index]
python/uv/_build_backend.py:46: error: Value of type "CompletedProcess[bytes]" is not indexable  [index]
Found 3 errors in 1 file (checked 6 source files)
```

So, I had to fix this problem by renaming the `result` var.

---

_@AlexWaygood reviewed on 2024-10-03 10:29_

---

_Review comment by @AlexWaygood on `python/uv/_build_backend.py`:40 on 2024-10-03 10:29_

typo: should be spelled `stdout`, not `sdtout`

---

_@sobolevn reviewed on 2024-10-03 10:38_

---

_Review comment by @sobolevn on `python/uv/_build_backend.py`:40 on 2024-10-03 10:38_

🤦‍♂️ 

---

_@AlexWaygood approved on 2024-10-03 10:41_

---

_Merged by @charliermarsh on 2024-10-03 12:05_

---

_Closed by @charliermarsh on 2024-10-03 12:05_

---

_Comment by @charliermarsh on 2024-10-03 12:05_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-10-03 12:05_

---
