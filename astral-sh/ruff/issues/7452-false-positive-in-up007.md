```yaml
number: 7452
title: False Positive in UP007
type: issue
state: closed
author: Matt-Ord
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-09-17T10:40:55Z
updated_at: 2023-09-19T03:37:40Z
url: https://github.com/astral-sh/ruff/issues/7452
synced_at: 2026-01-12T15:54:47Z
```

# False Positive in UP007

---

_@Matt-Ord_

False positive when using Union of unpacked TypeVarTuple. As far as I know there is no way to represent this without using Union[].

```python
class Collection(Protocol[*_B0]):
    def __iter__(self) -> Iterator[Union[*_B0]]:
        ...
```
ruff 0.0.290

---

_Label `bug` added by @charliermarsh on 2023-09-17 16:16_

---

_Comment by @charliermarsh on 2023-09-17 16:16_

Thanks!

---

_Label `accepted` added by @charliermarsh on 2023-09-17 16:16_

---

_Closed by @charliermarsh on 2023-09-19 03:37_

---
