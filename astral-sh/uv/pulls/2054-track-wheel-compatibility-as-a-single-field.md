```yaml
number: 2054
title: Track wheel compatibility as a single field
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dist
created_at: 2024-02-28T20:53:35Z
updated_at: 2024-03-01T11:55:56Z
url: https://github.com/astral-sh/uv/pull/2054
synced_at: 2026-01-12T16:04:51Z
```

# Track wheel compatibility as a single field

---

_@charliermarsh_

## Summary

Internal refactor to `PrioritizedDistribution` that I think should reduce the size? Although the motivation here is simplicity, not perf.

Instead of storing:

```rust
/// The highest-priority, installable wheel for the package version.
compatible_wheel: Option<(DistMetadata, TagPriority)>,
/// The most-relevant, incompatible wheel for the package version.
incompatible_wheel: Option<(DistMetadata, IncompatibleWheel)>,
```

We now store:

```rust
wheel: Option<(DistMetadata, WheelCompatibility)>,
```

Where `WheelCompatibility` is an enum of `TagPriority` or `IncompatibleWheel`.


---

_Label `internal` added by @charliermarsh on 2024-02-28 20:53_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-28 20:53_

---

_Merged by @charliermarsh on 2024-02-28 21:59_

---

_Closed by @charliermarsh on 2024-02-28 21:59_

---

_Branch deleted on 2024-02-28 21:59_

---

_@BurntSushi reviewed on 2024-03-01 11:55_

Nice simplification! I think some of Zanie's recent changes might have made this possible?

---
