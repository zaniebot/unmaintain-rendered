```yaml
number: 8672
title: "uv-resolver: simplify case analysis in ForkMap::get"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fork-map-simplification
created_at: 2024-10-29T17:26:18Z
updated_at: 2024-10-29T17:38:24Z
url: https://github.com/astral-sh/uv/pull/8672
synced_at: 2026-01-12T16:08:26Z
```

# uv-resolver: simplify case analysis in ForkMap::get

---

_@BurntSushi_

Previously, when doing a `uv pip` resolution, we would only return the
first entry in the map. But there should only ever be one entry, or else
we would have incompatible dependencies. So we can collapse the case
with the "one universal fork" case.

(I found this while doing some refactoring of how we handle forking, and
collapsing these cases simplifies some of that refactoring work.)


---

_Review requested from @konstin by @BurntSushi on 2024-10-29 17:26_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-10-29 17:26_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-10-29 17:26_

---

_Comment by @BurntSushi on 2024-10-29 17:38_

Just going to bring this in since it's such a small change, but happy to create follow-up PRs!

---

_Merged by @BurntSushi on 2024-10-29 17:38_

---

_Closed by @BurntSushi on 2024-10-29 17:38_

---

_Branch deleted on 2024-10-29 17:38_

---
