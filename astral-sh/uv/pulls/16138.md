```yaml
number: 16138
title: "Emit a message on `cache clean` and `prune` when lock is held"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/cache-message
created_at: 2025-10-06T14:46:51Z
updated_at: 2025-10-06T16:49:25Z
url: https://github.com/astral-sh/uv/pull/16138
synced_at: 2026-01-10T06:36:15Z
```

# Emit a message on `cache clean` and `prune` when lock is held

---

_Pull request opened by @zanieb on 2025-10-06 14:46_

Closes https://github.com/astral-sh/uv/issues/16112

```
❯ cargo run -q cache clean
Cache is currently in-use, waiting for other uv processes to finish (use `--force` to override)
```

Otherwise, `-v` is required to see we're waiting on a lock.

I'd rather do this than elevate the exclusive lock log message to something more verbose because this allows us to use a more specific message as appropriate for the situation. We also previously reduced the verbosity of arbitrary locks, e.g., see https://github.com/astral-sh/uv/issues/7489

---

_Label `enhancement` added by @zanieb on 2025-10-06 14:49_

---

_Marked ready for review by @zanieb on 2025-10-06 16:38_

---

_@konstin approved on 2025-10-06 16:41_

---

_Merged by @zanieb on 2025-10-06 16:49_

---

_Closed by @zanieb on 2025-10-06 16:49_

---

_Branch deleted on 2025-10-06 16:49_

---
