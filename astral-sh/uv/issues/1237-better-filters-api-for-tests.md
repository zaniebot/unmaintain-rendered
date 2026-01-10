---
number: 1237
title: Better filters API for tests 
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-02-02T18:35:33Z
updated_at: 2024-04-07T00:18:11Z
url: https://github.com/astral-sh/uv/issues/1237
synced_at: 2026-01-10T01:23:05Z
---

# Better filters API for tests 

---

_Issue opened by @konstin on 2024-02-02 18:35_

It would be nice to have a filters api that accepts both paths and strings and automatically refex escapes paths and isn't build around collecting a vec yourself. I imagine something like

```rust
let mut filters = Filters::default();
filters.add(workspace_root.path(), "[WORKSPACE_ROOT]");
filters.add("some.*pattern, "[SOME_PATTERN]");
```

and 

```rust
let mut filters = Filters::new();
filters.add("some.*pattern, "[SOME_PATTERN]");
filters.add_default_filters();
filters.add(workspace_root.path(), "[WORKSPACE_ROOT]");
```


---

_Label `internal` added by @konstin on 2024-02-02 18:35_

---

_Comment by @charliermarsh on 2024-04-07 00:18_

@zanieb improved this a lot. Gonna close.

---

_Closed by @charliermarsh on 2024-04-07 00:18_

---
