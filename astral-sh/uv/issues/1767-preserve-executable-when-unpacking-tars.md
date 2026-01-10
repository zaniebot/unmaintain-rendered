```yaml
number: 1767
title: Preserve executable when unpacking tars
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-02-20T17:12:26Z
updated_at: 2024-02-21T14:18:45Z
url: https://github.com/astral-sh/uv/issues/1767
synced_at: 2026-01-10T05:40:31Z
```

# Preserve executable when unpacking tars

---

_Issue opened by @konstin on 2024-02-20 17:12_

We preserve the executable bit an unpacking zips (#1743), we should do the same when unpacking source distributions, both async and sync.

---

_Label `bug` added by @konstin on 2024-02-20 17:12_

---

_Label `good first issue` added by @konstin on 2024-02-20 17:12_

---

_Comment by @MichaReiser on 2024-02-20 17:13_

Do you have any code pointers to the unpacking logic? I would find that helpful as a first time contributor.

---

_Comment by @konstin on 2024-02-20 17:18_

The streaming/async .tar.gz extraction: https://github.com/astral-sh/uv/blob/d6aaf7be3098ec381105c0e0db0391fd97db483d/crates/uv-extract/src/stream.rs#L103-L112
The sync version: https://github.com/astral-sh/uv/blob/9a720a87c842a070ecc58f5309c7ecc6a334da67/crates/uv-extract/src/sync.rs#L88-L107

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 00:26_

---

_Comment by @charliermarsh on 2024-02-21 00:29_

I'll fix this tonight, I've worked with these async unzip crates several times.

---

_Closed by @charliermarsh on 2024-02-21 14:18_

---
