```yaml
number: 7802
title: "no error when duplicated package in `dependencies`"
type: issue
state: open
author: DetachHead
labels:
  - needs-decision
assignees: []
created_at: 2024-09-30T05:01:55Z
updated_at: 2024-09-30T14:12:19Z
url: https://github.com/astral-sh/uv/issues/7802
synced_at: 2026-01-12T15:59:17Z
```

# no error when duplicated package in `dependencies`

---

_@DetachHead_

```toml
[project]
dependencies = [
    "typer>=0.12.5",
    "typer>=0.12.5",
]
```
it would be nice if uv reported an error in cases like this. in my case, i merged two branches that both added the same dependency, but ordered differently. this resulted in there being no merge conflict and the dependency being incorrectly specified twice

---

_Label `needs-decision` added by @charliermarsh on 2024-09-30 12:50_

---

_Comment by @zanieb on 2024-09-30 14:10_

Do other tools error here? i.e. Cargo and Poetry?

---

_Comment by @konstin on 2024-09-30 14:12_

We have to allow two requirements on the same package for cases when different marker expressions have different requirements, but we should warn when there is duplication without a marker or under the same marker, which can merged.

---
