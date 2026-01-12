```yaml
number: 555
title: "Use `u64` instead of `u32` in `Version` fields"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/u64
created_at: 2023-12-04T23:37:02Z
updated_at: 2023-12-05T02:00:56Z
url: https://github.com/astral-sh/uv/pull/555
synced_at: 2026-01-12T16:04:02Z
```

# Use `u64` instead of `u32` in `Version` fields

---

_@charliermarsh_

It turns out that it's not uncommon to use timestamps as patch versions (e.g., `20230628214621`). I believe this is the ISO 8601 "basic format". These can't be represented by a `u32`, so I think it makes sense to just bump to `u64` to remove this limitation.

---

_Label `bug` added by @charliermarsh on 2023-12-04 23:37_

---

_Review requested from @BurntSushi by @charliermarsh on 2023-12-04 23:37_

---

_Review requested from @konstin by @charliermarsh on 2023-12-04 23:37_

---

_@BurntSushi approved on 2023-12-05 01:59_

Ref 63f7f651900cfa2d5b9898285d2a017bf393663d where I bumped this from a `usize` to ` u32`.

One thought is whether it makes sense to bump all of these to `u64`. With that said, it probably makes sense to make these types bigger/more-conservative as you've done here until we have more solid measurements telling us it would help to do otherwise.

---

_Merged by @charliermarsh on 2023-12-05 02:00_

---

_Closed by @charliermarsh on 2023-12-05 02:00_

---

_Branch deleted on 2023-12-05 02:00_

---
