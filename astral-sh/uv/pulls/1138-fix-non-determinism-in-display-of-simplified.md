```yaml
number: 1138
title: Fix non-determinism in display of simplified ranges
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/fix-flake
created_at: 2024-01-26T20:20:59Z
updated_at: 2024-01-26T22:01:01Z
url: https://github.com/astral-sh/uv/pull/1138
synced_at: 2026-01-12T16:04:27Z
```

# Fix non-determinism in display of simplified ranges

---

_@zanieb_

Closes #863 by sorting versions before passing to `simplify`. There is no real reason this should have any affect because it's already a `BTreeSet` which should return a sorted iterator. However, it seems to work? :'(

---

_Label `testing` added by @zanieb on 2024-01-26 20:21_

---

_Review requested from @BurntSushi by @zanieb on 2024-01-26 21:40_

---

_Comment by @zanieb on 2024-01-26 21:50_

🤷‍♀️ 
<img width="313" alt="Screenshot 2024-01-26 at 15 50 36" src="https://github.com/astral-sh/puffin/assets/2586601/13c9b85c-6540-421c-82f8-ecb0624e0a30">


---

_Comment by @charliermarsh on 2024-01-26 21:51_

It could be a bug in the comparison method for versions.

---

_Comment by @zanieb on 2024-01-26 22:01_

I thought about that too and the timing of #863 is slightly suspicious compared to #373 but I think this fix is wrong :(

---

_Closed by @zanieb on 2024-01-26 22:01_

---
