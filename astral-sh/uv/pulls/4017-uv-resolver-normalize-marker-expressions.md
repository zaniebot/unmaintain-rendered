```yaml
number: 4017
title: "uv-resolver: normalize marker expressions"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/normalize-markers
created_at: 2024-06-04T17:28:57Z
updated_at: 2024-06-04T17:45:54Z
url: https://github.com/astral-sh/uv/pull/4017
synced_at: 2026-01-10T13:54:02Z
```

# uv-resolver: normalize marker expressions

---

_Pull request opened by @BurntSushi on 2024-06-04 17:28_

This is a quick fix for some flaky tests where the output in the lock
file isn't stable because marker expressions can be combined in a
non-deterministic order.

I believe there is ongoing work to simplify marker expressions which
will help here, but I think some kind of normalization is still
ultimately needed to guarantee consistent output.

I first noticed the flaky test in:
https://github.com/astral-sh/uv/pull/4015


---

_Review requested from @ibraheemdev by @BurntSushi on 2024-06-04 17:33_

---

_@ibraheemdev approved on 2024-06-04 17:34_

---

_Merged by @BurntSushi on 2024-06-04 17:45_

---

_Closed by @BurntSushi on 2024-06-04 17:45_

---

_Branch deleted on 2024-06-04 17:45_

---
