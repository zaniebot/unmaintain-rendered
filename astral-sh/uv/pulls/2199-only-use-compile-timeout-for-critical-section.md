```yaml
number: 2199
title: Only use compile timeout for critical section
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/different-compile-timeout
created_at: 2024-03-05T10:21:58Z
updated_at: 2024-03-05T16:20:05Z
url: https://github.com/astral-sh/uv/pull/2199
synced_at: 2026-01-10T14:54:43Z
```

# Only use compile timeout for critical section

---

_Pull request opened by @konstin on 2024-03-05 10:21_

Follow-up to #2086: Don't use timeouts for the entire workers, but only for the section that's about communicating with the (potentially broken) `python` subprocess. I've also raised the timeout to 60s.


---

_Review requested from @BurntSushi by @konstin on 2024-03-05 10:21_

---

_@BurntSushi approved on 2024-03-05 12:11_

Nice, this LGTM! Thanks!

---

_Merged by @konstin on 2024-03-05 16:20_

---

_Closed by @konstin on 2024-03-05 16:20_

---

_Branch deleted on 2024-03-05 16:20_

---
