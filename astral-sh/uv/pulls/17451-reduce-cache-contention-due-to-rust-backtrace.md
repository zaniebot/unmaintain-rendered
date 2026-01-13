```yaml
number: 17451
title: "Reduce cache contention due to `RUST_BACKTRACE` environment variable"
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/cache-standardize-rust-backtrace
created_at: 2026-01-13T22:48:13Z
updated_at: 2026-01-13T23:19:56Z
url: https://github.com/astral-sh/uv/pull/17451
synced_at: 2026-01-13T23:35:46Z
```

# Reduce cache contention due to `RUST_BACKTRACE` environment variable

---

_@zanieb_

This is included in the Rust cache key hash because it is prefixed with `RUST_` but has no affect on compilation. This invalidates caches that could be shared across jobs.

---

_Label `internal` added by @zanieb on 2026-01-13 22:48_

---

_Renamed from "Add RUST_BACKTRACE to workflow env for consistent cache keys" to "Reduce cache contention due to `RUST_BACKTRACE` environment variable" by @zanieb on 2026-01-13 22:51_

---

_Marked ready for review by @zanieb on 2026-01-13 23:19_

---
