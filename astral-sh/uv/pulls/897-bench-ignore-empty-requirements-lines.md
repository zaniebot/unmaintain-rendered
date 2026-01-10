```yaml
number: 897
title: "bench: ignore empty requirements lines"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/bench-fix
created_at: 2024-01-12T14:30:21Z
updated_at: 2024-01-12T14:40:26Z
url: https://github.com/astral-sh/uv/pull/897
synced_at: 2026-01-10T15:39:02Z
```

# bench: ignore empty requirements lines

---

_Pull request opened by @BurntSushi on 2024-01-12 14:30_

In particular, this script previously choked on the `home-assistant.in`
requirements file because it contains many empty lines.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-12 14:31_

---

_@zanieb approved on 2024-01-12 14:31_

Nit: `and line.strip()` is sufficient and Pythonic but this is fine too :) I like explicit

---

_Label `bug` added by @charliermarsh on 2024-01-12 14:34_

---

_Merged by @BurntSushi on 2024-01-12 14:39_

---

_Closed by @BurntSushi on 2024-01-12 14:39_

---

_Branch deleted on 2024-01-12 14:39_

---

_Comment by @BurntSushi on 2024-01-12 14:40_

> Nit: `and line.strip()` is sufficient and Pythonic but this is fine too :) I like explicit

Yeah I seem to recall not being a fan of implicit boolean conversions. I think I used to have an example where that could go awry, but that part of my brain is creaky and full of cobwebs.

---
