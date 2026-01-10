```yaml
number: 6078
title: "Store `environment-markers` in solve order"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
assignees: []
merged: true
base: main
head: charlie/vec
created_at: 2024-08-14T02:46:53Z
updated_at: 2024-08-14T13:35:03Z
url: https://github.com/astral-sh/uv/pull/6078
synced_at: 2026-01-10T13:09:50Z
```

# Store `environment-markers` in solve order

---

_Pull request opened by @charliermarsh on 2024-08-14 02:46_

## Summary

Right now, we store the environment markers in a `BTreeSet` -- so they're sorted, but the sort doesn't really tell us anything. I think we should instead store them in the order in which we solved. I thought this might fix an instability (it didn't), but I think it's still good to ensure we solve in the same order.

I also changed from `Option<Vec>` to just `Vec`, since there was no distinction between `None` and empty.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-14 02:46_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-14 02:46_

---

_Label `internal` added by @charliermarsh on 2024-08-14 02:47_

---

_Label `preview` added by @charliermarsh on 2024-08-14 02:47_

---

_@ibraheemdev approved on 2024-08-14 03:10_

This makes sense to me.

---

_Merged by @charliermarsh on 2024-08-14 13:20_

---

_Closed by @charliermarsh on 2024-08-14 13:20_

---

_Branch deleted on 2024-08-14 13:20_

---

_@BurntSushi reviewed on 2024-08-14 13:35_

SGTM too.

---
