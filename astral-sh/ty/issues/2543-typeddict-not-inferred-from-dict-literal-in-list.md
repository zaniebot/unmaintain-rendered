```yaml
number: 2543
title: TypedDict not inferred from dict literal in list comprehension
type: issue
state: closed
author: gaborbernat
labels: []
assignees: []
created_at: 2026-01-17T05:43:01Z
updated_at: 2026-01-17T06:26:08Z
url: https://github.com/astral-sh/ty/issues/2543
synced_at: 2026-01-17T07:05:09Z
```

# TypedDict not inferred from dict literal in list comprehension

---

_@gaborbernat_

### Summary

Dict literals in list comprehensions are not recognized as TypedDict, even when the iteration variable has the correct type.


## Minimal reproduction

```python
from typing import Literal, TypedDict

PackageType = Literal["Python", "OCI", "JVM"]

class RepoReq(TypedDict):
    name: str
    packageType: PackageType

def make_repos() -> list[RepoReq]:
    pkg_types: list[PackageType] = ["Python", "JVM"]
    return [{"name": "", "packageType": t} for t in pkg_types]
```

## Error

```
error[invalid-return-type]: Return type does not match returned value
  --> issue.py:11:21
   |
11 | def make_repos() -> list[RepoReq]:
   |                     ------------- Expected `list[RepoReq]` because of return type
12 |     pkg_types: list[PackageType] = ["Python", "JVM"]
13 |     return [{"name": "", "packageType": t} for t in pkg_types]
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `list[RepoReq]`, found `list[dict[Unknown | str, Unknown | str] | Unknown]`
```

## Expected behavior

The dict literal `{"name": "", "packageType": t}` should be inferred as `RepoReq` since all keys and value types match.


### Version

0.0.12

---

_Comment by @ibraheemdev on 2026-01-17 06:26_

Thanks for the report! This is a known issue, and will be resolved by https://github.com/astral-sh/ruff/pull/22564.

---

_Closed by @ibraheemdev on 2026-01-17 06:26_

---
