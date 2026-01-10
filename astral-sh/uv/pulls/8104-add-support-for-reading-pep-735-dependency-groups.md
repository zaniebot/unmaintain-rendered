```yaml
number: 8104
title: Add support for reading PEP 735 dependency groups
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - compatibility
assignees: []
merged: true
base: tracking/735
head: zb/735-read
created_at: 2024-10-10T18:41:28Z
updated_at: 2024-10-16T21:34:48Z
url: https://github.com/astral-sh/uv/pull/8104
synced_at: 2026-01-10T12:54:03Z
```

# Add support for reading PEP 735 dependency groups

---

_Pull request opened by @zanieb on 2024-10-10 18:41_

Part of #8090 

As a basic first step, we parse these groups defined in `pyproject.toml` files.

---

_Label `compatibility` added by @zanieb on 2024-10-10 18:41_

---

_Label `internal` added by @zanieb on 2024-10-10 18:41_

---

_Marked ready for review by @zanieb on 2024-10-10 21:19_

---

_Review comment by @mkniewallner on `crates/uv-workspace/src/pyproject.rs`:1058 on 2024-10-10 21:22_

```suggestion
    /// A dependency in `dependency-groups.{0}`.
```

---

_Review comment by @mkniewallner on `crates/uv-workspace/src/pyproject_mut.rs`:498 on 2024-10-10 21:22_

```suggestion
        // Check `dependency-groups`.
```

---

_@mkniewallner reviewed on 2024-10-10 21:23_

---

_@charliermarsh approved on 2024-10-11 08:33_

---

_@konstin reviewed on 2024-10-15 08:03_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:47 on 2024-10-15 08:03_

I'd put it in a newtype so we can implement a `get_group_resolved` on top of it. https://github.com/PyO3/pyproject-toml-rs/pull/24 is good reference implementation, we can reuse it.

---

_@zanieb reviewed on 2024-10-15 13:55_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:47 on 2024-10-15 13:55_

I'll add support for the `include-group` syntax separately. This just copies our existing patterns.

I can hold of on merging though since it'd break people using the syntax.

---

_@zanieb reviewed on 2024-10-15 17:46_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:47 on 2024-10-15 17:46_

Honestly pretty hesitant to change the pattern here? We probably want to change all the structures separately if you want a different approach.

---

_@konstin reviewed on 2024-10-16 09:20_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:47 on 2024-10-16 09:20_

We're using both styles in uv, it's not that much of a difference in the end

---

_Merged by @zanieb on 2024-10-16 21:34_

---

_Closed by @zanieb on 2024-10-16 21:34_

---

_Branch deleted on 2024-10-16 21:34_

---
