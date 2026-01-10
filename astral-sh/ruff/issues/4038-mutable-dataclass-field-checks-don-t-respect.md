```yaml
number: 4038
title: "Mutable dataclass field checks don't respect immutable annotations"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-04-20T03:30:33Z
updated_at: 2023-04-20T20:02:14Z
url: https://github.com/astral-sh/ruff/issues/4038
synced_at: 2026-01-10T11:09:46Z
```

# Mutable dataclass field checks don't respect immutable annotations

---

_Issue opened by @charliermarsh on 2023-04-20 03:30_

_Originally posted by @andersk in https://github.com/charliermarsh/ruff/issues/3877#issuecomment-1515536738_
            

---

_Label `bug` added by @charliermarsh on 2023-04-20 03:30_

---

_Comment by @dhruvmanila on 2023-04-20 05:20_

I can take this up :)

Am I correct to assume that the said helper function in the original comment should be refactored to `ruff_python_semantic`? This is because it's dependent on `Context`. 

---

_Comment by @Pentusha on 2023-04-20 10:52_

I think my case is suitable for this issue:

error message:
```
 edgeql_qb/render/types.py:10:27: RUF009 Do not perform function call `FrozenDict` in dataclass defaults
```
 
immutable dataclass with immutable field:
```python
@dataclass(slots=True, frozen=True)
class RenderedQuery:
    query: str = ''
    context: FrozenDict = FrozenDict()
```

Hashable (Immutable) FrozenDict code [here](https://github.com/Pentusha/edgeql-qb/blob/master/edgeql_qb/frozendict.py#L7-L38).
For me it seems like false positive.

Ref: Pentusha/edgeql-qb#92

---

_Comment by @dhruvmanila on 2023-04-20 11:14_

@Pentusha This comes from `RUF009` while this issue is to track `RUF008`. Yours is a valid case as well but it might be difficult to detect due to the lack of type inference.

---

_Closed by @charliermarsh on 2023-04-20 20:02_

---
