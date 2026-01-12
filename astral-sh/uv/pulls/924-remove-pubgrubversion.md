```yaml
number: 924
title: "Remove `PubGrubVersion`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/version
created_at: 2024-01-15T04:49:38Z
updated_at: 2024-01-15T13:51:13Z
url: https://github.com/astral-sh/uv/pull/924
synced_at: 2026-01-12T16:04:17Z
```

# Remove `PubGrubVersion`

---

_@charliermarsh_

## Summary

I'm running into some annoyances converting `&Version` to `&PubGrubVersion` (which is just a wrapper type around `Version`), and I realized... We don't even need `PubGrubVersion`?

The reason we "need" it today is due to the orphan trait rule: `Version` is defined in `pep440_rs`, but we want to `impl pubgrub::version::Version for Version` in the resolver crate.

Instead of introducing a new type here, which leads to a lot of awkwardness around conversion and API isolation, what if we instead just implement `pubgrub::version::Version` in `pep440_rs` via a feature? That way, we can just use `Version` everywhere without any confusion and conversion for the wrapper type.

---

_Renamed from "Remove PubGrubVersion" to "Remove `PubGrubVersion`" by @charliermarsh on 2024-01-15 04:49_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-15 04:49_

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 04:49_

---

_@charliermarsh reviewed on 2024-01-15 04:52_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:423 on 2024-01-15 04:52_

Nice to get rid of a lot of stuff like this: clone, then `PubGrubVersion::from`, then using `&version`...

---

_Label `internal` added by @charliermarsh on 2024-01-15 04:54_

---

_@konstin approved on 2024-01-15 08:41_

That is much more convenient

---

_Merged by @charliermarsh on 2024-01-15 13:51_

---

_Closed by @charliermarsh on 2024-01-15 13:51_

---

_Branch deleted on 2024-01-15 13:51_

---
