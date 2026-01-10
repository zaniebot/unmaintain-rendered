```yaml
number: 7432
title: Cache ecosystem tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cache-ecosystem-checks
created_at: 2024-09-16T16:19:51Z
updated_at: 2024-09-18T09:31:01Z
url: https://github.com/astral-sh/uv/pull/7432
synced_at: 2026-01-10T12:53:47Z
```

# Cache ecosystem tests

---

_Pull request opened by @konstin on 2024-09-16 16:19_

Use a cache in `target` for ecosystem checks. I had frequent timeouts with the transformers test, which this fixes. Locally, the transformers test alone goes from 15s to 2s on my machine.

By using `uv cache prune --ci`, the runtime would go up from 2s to 5.5s, with the cache shrinking from 425MB to 415MB. This is due to uv not being optimized for the case where we have a warm cache but no lockfile, so in this test we're doing what we otherwise don't recommend and don't prune the cache.

Fixes #6330


---

_Label `internal` added by @konstin on 2024-09-16 16:19_

---

_@BurntSushi approved on 2024-09-16 16:25_

SGTM

---

_Marked ready for review by @konstin on 2024-09-17 11:50_

---

_Comment by @konstin on 2024-09-17 11:52_

I trimmed it down to not touch the `--cache-dir` or we would have to change all the `requirements.txt` snapshots containing `--cache-dir`

---

_Merged by @konstin on 2024-09-18 09:31_

---

_Closed by @konstin on 2024-09-18 09:31_

---

_Branch deleted on 2024-09-18 09:31_

---
