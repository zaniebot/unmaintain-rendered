```yaml
number: 11447
title: "Insert `pyproject.toml` `[dependency-groups]` entries in sorted order"
type: issue
state: closed
author: woutervh
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T15:08:45Z
updated_at: 2025-02-18T18:12:52Z
url: https://github.com/astral-sh/uv/issues/11447
synced_at: 2026-01-12T16:00:37Z
```

# Insert `pyproject.toml` `[dependency-groups]` entries in sorted order

---

_@woutervh_

### Summary

```
> uv --version
uv 0.5.30
```


create a new package,   and add some dependencies in various dependency-groups:

```
uv init demo --package    
cd demo
uv add pytest --group testing        
uv add mypy --group typing   
uv add ruff --group dev  
```


This leads to ordering in the way they were added:
```
[dependency-groups]
testing = [
    "pytest>=8.3.4",
]
typing = [
    "mypy>=1.15.0",
]
dev = [
    "ruff>=0.9.6",
]
```


cleaner would be to order the keys:
```
[dependency-groups]
dev = [
    "ruff>=0.9.6",
]
testing = [
    "pytest>=8.3.4",
]
typing = [
    "mypy>=1.15.0",
]
```




### Example

_No response_

---

_Label `enhancement` added by @woutervh on 2025-02-12 15:08_

---

_Comment by @zanieb on 2025-02-12 15:12_

Seems reasonable to do what we do for dependencies — insertions should be sorted if the list is already sorted. Otherwise, they should not to avoid churn.

---

_Renamed from "sort dependency groups in pyproject.toml" to "Insert `pyproject.toml` `[dependency-groups]` entries in sorted order" by @zanieb on 2025-02-16 19:53_

---

_Closed by @jtfmumm on 2025-02-18 18:12_

---

_Closed by @jtfmumm on 2025-02-18 18:12_

---
