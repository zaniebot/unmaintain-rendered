```yaml
number: 2388
title: "Stop looking for `-p` in PATH"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: konsti/dont-do-path-lookups
created_at: 2024-03-12T17:49:42Z
updated_at: 2024-04-05T14:32:57Z
url: https://github.com/astral-sh/uv/pull/2388
synced_at: 2026-01-10T14:43:31Z
```

# Stop looking for `-p` in PATH

---

_Pull request opened by @konstin on 2024-03-12 17:49_

See #2386 for the motivation and semantics. We keep only two behaviours for `-p`, we have an absolute or relative path (containing a system path separator) or `-p <implementation><version>`.

Open question: Should `python3.10` or `3.10` force cpython 3.10 or is any python implementation acceptable? My assumption right now is that i assume that a user specifying nothing wouldn't like it if we picked pypy, rather we should force cpython and make pypy explicit.

I will add tests tomorrow, please already give the semantics a look.

---

_Review requested from @charliermarsh by @konstin on 2024-03-12 17:49_

---

_Review requested from @zanieb by @konstin on 2024-03-12 17:49_

---

_Comment by @charliermarsh on 2024-03-12 18:42_

Please don't merge this yet. I haven't had a chance to react to the discussion in #2386. I don't consider it decided yet!

---

_Closed by @konstin on 2024-04-05 14:32_

---
