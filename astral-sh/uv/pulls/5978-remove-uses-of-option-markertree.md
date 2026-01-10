```yaml
number: 5978
title: "Remove uses of `Option<MarkerTree>`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/marker-smart-constructors
created_at: 2024-08-09T20:27:29Z
updated_at: 2024-08-10T17:23:31Z
url: https://github.com/astral-sh/uv/pull/5978
synced_at: 2026-01-10T13:31:54Z
```

# Remove uses of `Option<MarkerTree>`

---

_Pull request opened by @ibraheemdev on 2024-08-09 20:27_

## Summary

Follow up to https://github.com/astral-sh/uv/pull/5898. This should fix some of the failures in https://github.com/astral-sh/uv/pull/5887 where `uv lock --locked` is failing due to `Some(true)` and `None` markers not comparing equal.

---

_Label `internal` added by @ibraheemdev on 2024-08-09 20:27_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-08-09 20:27_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:3119 on 2024-08-09 20:31_

Unrelated to this changed, but also noticed that the invariant here is reversed. This should be fixed as part of https://github.com/astral-sh/uv/issues/5562.

---

_@ibraheemdev reviewed on 2024-08-09 20:31_

---

_@charliermarsh reviewed on 2024-08-10 03:58_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:143 on 2024-08-10 03:58_

Do we need to clone here in order to support unwrapping to default?

---

_@charliermarsh approved on 2024-08-10 03:59_

This looks reasonable to me.

---

_@ibraheemdev reviewed on 2024-08-10 04:04_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:143 on 2024-08-10 04:04_

We used to clone inside `add_dependency` anyways. Also a `MarkerTree` is just a `usize` now, it could even implement `Copy`.

---

_@charliermarsh reviewed on 2024-08-10 04:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:143 on 2024-08-10 04:07_

👍 Yeah sorry saw that afterwards, they're all interned now, right?

---

_@ibraheemdev reviewed on 2024-08-10 04:22_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:143 on 2024-08-10 04:22_

Yeah.

---

_Merged by @ibraheemdev on 2024-08-10 17:23_

---

_Closed by @ibraheemdev on 2024-08-10 17:23_

---

_Branch deleted on 2024-08-10 17:23_

---
