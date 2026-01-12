```yaml
number: 3113
title: "Use `BoolishValueParser` for boolean environment variables"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/bool
created_at: 2024-04-18T03:48:12Z
updated_at: 2024-04-19T04:17:44Z
url: https://github.com/astral-sh/uv/pull/3113
synced_at: 2026-01-12T16:05:26Z
```

# Use `BoolishValueParser` for boolean environment variables

---

_@charliermarsh_

## Summary

Right now, we only accept _exactly `UV_NATIVE_TLS=true` and `UV_NATIVE_TLS=false`. `BoolishValueParser` accepts a wider range of values:

```rust
/// True values are `y`, `yes`, `t`, `true`, `on`, and `1`.
pub(crate) const TRUE_LITERALS: [&str; 6] = ["y", "yes", "t", "true", "on", "1"];

/// False values are `n`, `no`, `f`, `false`, `off`, and `0`.
pub(crate) const FALSE_LITERALS: [&str; 6] = ["n", "no", "f", "false", "off", "0"];
```

I tend to use `0` and `1` personally so this surprised me.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-18 03:48_

---

_Review requested from @konstin by @charliermarsh on 2024-04-18 03:48_

---

_Marked ready for review by @charliermarsh on 2024-04-18 03:48_

---

_Label `cli` added by @charliermarsh on 2024-04-18 03:48_

---

_@zanieb approved on 2024-04-18 04:10_

---

_Merged by @charliermarsh on 2024-04-18 04:37_

---

_Closed by @charliermarsh on 2024-04-18 04:37_

---

_Branch deleted on 2024-04-18 04:37_

---
