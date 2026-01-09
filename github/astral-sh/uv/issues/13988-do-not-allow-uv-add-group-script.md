---
number: 13988
title: "Do not allow `uv add --group ... --script`"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-06-12T13:07:14Z
updated_at: 2025-06-12T14:45:13Z
url: https://github.com/astral-sh/uv/issues/13988
synced_at: 2026-01-07T13:12:18-06:00
---

# Do not allow `uv add --group ... --script`

---

_Issue opened by @zanieb on 2025-06-12 13:07_

uv will happily do

```
uv add --group science numpy --script main.py
```

to create

```python
# /// script
# requires-python = ">=3.13"
#
# [dependency-groups]
# science = [
#     "numpy",
# ]
# ///
```

it doesn't like

```
uv sync --group science --script main.py
```

_Originally posted by @konstin in https://github.com/astral-sh/uv/pull/13735#discussion_r2142078192_

This isn't allowed by the specification.
            

---

_Referenced in [astral-sh/uv#13735](../../astral-sh/uv/pulls/13735.md) on 2025-06-12 13:07_

---

_Label `bug` added by @Gankra on 2025-06-12 13:12_

---

_Label `good first issue` added by @Gankra on 2025-06-12 13:12_

---

_Comment by @Gankra on 2025-06-12 13:15_

For anyone who wants to fix this, you should probably just add an error here if dependency_type is DependencyType::Group

https://github.com/astral-sh/uv/blob/87ab57e902c9009b47f22d951acf631b515e3bed/crates/uv/src/commands/project/add.rs#L125-L131

---

_Referenced in [astral-sh/uv#13997](../../astral-sh/uv/pulls/13997.md) on 2025-06-12 13:57_

---

_Closed by @Gankra on 2025-06-12 14:45_

---
