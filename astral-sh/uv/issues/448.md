```yaml
number: 448
title: "Track which index a `File` came from"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-11-19T10:39:40Z
updated_at: 2023-11-20T08:48:17Z
url: https://github.com/astral-sh/uv/issues/448
synced_at: 2026-01-10T05:40:31Z
```

# Track which index a `File` came from

---

_Issue opened by @konstin on 2023-11-19 10:39_

Currently, we don't track where a file came from, instead, we drop that information:

https://github.com/astral-sh/puffin/blob/751f7fa9c6f81ce785cf79c11357fbc83f70315c/crates/puffin-client/src/client.rs#L173-L174

We should track that information so that for every package name we know the index we queried the files for. This will be helpful for caching, debugging and giving the user information about the resolution

---

_Assigned to @konstin by @konstin on 2023-11-19 12:04_

---

_Closed by @konstin on 2023-11-20 08:48_

---
