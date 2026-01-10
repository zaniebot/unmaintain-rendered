```yaml
number: 4271
title: Allow normalization to completely eliminate markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/norm
created_at: 2024-06-12T14:08:20Z
updated_at: 2024-06-12T14:25:45Z
url: https://github.com/astral-sh/uv/pull/4271
synced_at: 2026-01-10T13:54:02Z
```

# Allow normalization to completely eliminate markers

---

_Pull request opened by @charliermarsh on 2024-06-12 14:08_

## Summary

`normalize` now takes an owned value and returns `Option<MarkerTree>`, such that if any sub-expression evaluates to `true`, we can normalize out the entire marker.

Closes https://github.com/astral-sh/uv/issues/4267.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-12 14:08_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-12 14:08_

---

_Label `bug` added by @charliermarsh on 2024-06-12 14:08_

---

_@charliermarsh reviewed on 2024-06-12 14:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:99 on 2024-06-12 14:09_

I split these into separate branches. There's a little bit of duplication, but it makes things easier with ownership (and the branches now have more deviations than before between the two cases).

---

_@BurntSushi approved on 2024-06-12 14:21_

Nice!

---

_Merged by @charliermarsh on 2024-06-12 14:25_

---

_Closed by @charliermarsh on 2024-06-12 14:25_

---

_Branch deleted on 2024-06-12 14:25_

---
