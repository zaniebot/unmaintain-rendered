```yaml
number: 11195
title: Remove warnings for missing lower bounds
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/lower-warn
created_at: 2025-02-03T20:44:38Z
updated_at: 2025-02-03T22:03:34Z
url: https://github.com/astral-sh/uv/pull/11195
synced_at: 2026-01-12T16:09:43Z
```

# Remove warnings for missing lower bounds

---

_@zanieb_

These are noisy relative to the effect they have on the user. It seems better to prioritize hints on poor resolutions. Notably, it seems hard to make these "not noisy" ref #11091.

Does not include the "lowest" resolution mode, in which lower bounds are critical.


---

_Label `cli` added by @zanieb on 2025-02-03 20:44_

---

_Review requested from @konstin by @zanieb on 2025-02-03 21:40_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-03 21:40_

---

_Marked ready for review by @zanieb on 2025-02-03 21:41_

---

_@Gankra approved on 2025-02-03 21:55_

wow that's a lot of code removed

---

_@charliermarsh approved on 2025-02-03 21:58_

I'm in favor of this. I think these have value, but that it's overall a better tradeoff to omit them.

---

_Merged by @zanieb on 2025-02-03 22:03_

---

_Closed by @zanieb on 2025-02-03 22:03_

---

_Branch deleted on 2025-02-03 22:03_

---
