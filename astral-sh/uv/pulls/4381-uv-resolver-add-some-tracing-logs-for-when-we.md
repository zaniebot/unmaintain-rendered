```yaml
number: 4381
title: "uv-resolver: add some tracing logs for when we filter requirements"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/universal-filter-logs
created_at: 2024-06-18T13:56:29Z
updated_at: 2024-06-18T15:15:17Z
url: https://github.com/astral-sh/uv/pull/4381
synced_at: 2026-01-10T13:54:02Z
```

# uv-resolver: add some tracing logs for when we filter requirements

---

_Pull request opened by @BurntSushi on 2024-06-18 13:56_

Specifically, these are emitted when requirements fail to satisfy
`Requires-Python` or the markers associated with the current fork in the
resolver.

Closes #4373


---

_Review requested from @zanieb by @BurntSushi on 2024-06-18 13:56_

---

_Comment by @zanieb on 2024-06-18 14:44_

Did you check for how noisy this is?

---

_Comment by @BurntSushi on 2024-06-18 14:58_

> Did you check for how noisy this is?

I tried locking with the `pyproject.toml` in #4133, and the result is that about 10% of all `TRACE` logs correspond to this addition. And that also reflects about 5% of total `DEBUG` logs. So I'd say this seems fine. It does seem like pretty useful debugging info.

---

_@zanieb approved on 2024-06-18 15:06_

---

_Merged by @BurntSushi on 2024-06-18 15:15_

---

_Closed by @BurntSushi on 2024-06-18 15:15_

---

_Branch deleted on 2024-06-18 15:15_

---
