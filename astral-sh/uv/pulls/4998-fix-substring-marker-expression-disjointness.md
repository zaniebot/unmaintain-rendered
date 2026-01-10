```yaml
number: 4998
title: Fix substring marker expression disjointness checks
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: ibraheem/substring-disjointness
created_at: 2024-07-11T21:10:51Z
updated_at: 2024-07-12T15:21:22Z
url: https://github.com/astral-sh/uv/pull/4998
synced_at: 2026-01-10T13:42:52Z
```

# Fix substring marker expression disjointness checks

---

_Pull request opened by @ibraheemdev on 2024-07-11 21:10_

## Summary

Noticed a bug here, `'a' in env` and `env not in 'a'` are not disjoint given `env == 'ab'`.

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-11 21:10_

---

_Label `bug` added by @ibraheemdev on 2024-07-11 21:11_

---

_Label `preview` added by @ibraheemdev on 2024-07-11 21:11_

---

_Label `preview` removed by @ibraheemdev on 2024-07-11 21:11_

---

_@charliermarsh approved on 2024-07-12 01:28_

---

_@charliermarsh reviewed on 2024-07-12 01:28_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:52 on 2024-07-12 01:28_

Can you include an example here, as you did in the PR summary?

---

_@konstin approved on 2024-07-12 07:22_

---

_@BurntSushi approved on 2024-07-12 12:13_

Nice catch.

---

_Merged by @ibraheemdev on 2024-07-12 15:21_

---

_Closed by @ibraheemdev on 2024-07-12 15:21_

---

_Branch deleted on 2024-07-12 15:21_

---
