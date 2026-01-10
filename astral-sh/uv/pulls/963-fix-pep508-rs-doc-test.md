```yaml
number: 963
title: "Fix `pep508_rs` doc test"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/test-features
created_at: 2024-01-18T11:45:36Z
updated_at: 2024-01-18T14:24:32Z
url: https://github.com/astral-sh/uv/pull/963
synced_at: 2026-01-10T15:39:03Z
```

# Fix `pep508_rs` doc test

---

_Pull request opened by @konstin on 2024-01-18 11:45_

Since nextest does not run doctests, this did not show up on CI.

---

_Comment by @BurntSushi on 2024-01-18 13:58_

Hmmm I don't think this `--features all-tests` does the same thing as `--all-features`. I think you'd need to add an `all-tests` feature to every crate duplicating all of the feature names in that crate (sans any debug features).

---

_Renamed from "Use a dedicated `all-tests` features instead of `all-features`" to "Fix `pep508_rs` doc test" by @konstin on 2024-01-18 14:23_

---

_Comment by @konstin on 2024-01-18 14:23_

I've trimmed this down to only fix the `pep508_rs` doc test

---

_Merged by @konstin on 2024-01-18 14:24_

---

_Closed by @konstin on 2024-01-18 14:24_

---

_Branch deleted on 2024-01-18 14:24_

---
