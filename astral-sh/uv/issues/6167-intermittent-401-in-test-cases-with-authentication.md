```yaml
number: 6167
title: Intermittent 401 in test cases with authentication
type: issue
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-08-17T02:03:54Z
updated_at: 2024-08-18T21:04:07Z
url: https://github.com/astral-sh/uv/issues/6167
synced_at: 2026-01-12T15:59:01Z
```

# Intermittent 401 in test cases with authentication

---

_@zanieb_

We're seeing intermittent HTTP 401 Unauthorized errors in our test suite. 

---

_Label `internal` added by @zanieb on 2024-08-17 02:03_

---

_Label `testing` added by @zanieb on 2024-08-17 02:03_

---

_Comment by @zanieb on 2024-08-18 14:01_

`lock_redact_https` in https://github.com/astral-sh/uv/actions/runs/10441180546/job/28912009907?pr=6172 failed with

```
        6 │+thread 'uv-resolver' panicked at [WORKSPACE]/crates/uv-resolver/src/resolver/mod.rs:274:33:
        7 │+called `Result::unwrap()` on an `Err` value: Err(ChannelClosed)
        8 │+note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Closed by @zanieb on 2024-08-18 21:04_

---
