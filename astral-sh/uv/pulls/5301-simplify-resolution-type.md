```yaml
number: 5301
title: "Simplify `Resolution` type"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/save-forks-in-lockfile
created_at: 2024-07-22T18:03:48Z
updated_at: 2024-07-23T18:54:03Z
url: https://github.com/astral-sh/uv/pull/5301
synced_at: 2026-01-10T13:37:23Z
```

# Simplify `Resolution` type

---

_Pull request opened by @konstin on 2024-07-22 18:03_

Looking at how to merge identical forks, i found that the `Resolution` can be simplified by treating it as a nodes and edges store (plus pins, they are separate since they are per name-version, not per (virtual-)package-version). This should also make #5294 more apparent, which i didn't touch here.

I additionally added some doc comments to the `Resolution` types.

---

_Label `internal` added by @konstin on 2024-07-22 18:03_

---

_Review requested from @BurntSushi by @konstin on 2024-07-22 18:03_

---

_@charliermarsh reviewed on 2024-07-22 18:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2367 on 2024-07-22 18:08_

Should we rename this field to `nodes`?

---

_@charliermarsh approved on 2024-07-22 18:10_

---

_@charliermarsh reviewed on 2024-07-22 18:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2367 on 2024-07-22 18:10_

E.g. "nodes and edges" rather than "packages and edges"

---

_Merged by @charliermarsh on 2024-07-22 19:10_

---

_Closed by @charliermarsh on 2024-07-22 19:10_

---

_Branch deleted on 2024-07-22 19:10_

---

_@BurntSushi reviewed on 2024-07-23 18:54_

Interesting, yeah, I think this is simpler. I wonder if our access patterns changed since I originally wrote this type.

---
