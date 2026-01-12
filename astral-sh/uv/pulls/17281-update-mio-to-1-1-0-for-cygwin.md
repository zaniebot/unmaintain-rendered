```yaml
number: 17281
title: Update mio to 1.1.0 (for Cygwin)
type: pull_request
state: merged
author: lazka
labels:
  - internal
  - dependencies
assignees: []
merged: true
base: main
head: update-mio-cygwin
created_at: 2026-01-01T11:02:15Z
updated_at: 2026-01-06T09:51:35Z
url: https://github.com/astral-sh/uv/pull/17281
synced_at: 2026-01-12T16:12:41Z
```

# Update mio to 1.1.0 (for Cygwin)

---

_@lazka_

## Summary

This makes it possible to build "uv-build" under Cygwin. See https://github.com/tokio-rs/mio/pull/1871 for the required upstream change from mio.

Note that "uv" itself still fails to build with this.

## Test Plan

I have a patched cygwin build here https://packages.msys2.org/packages/python-uv-build which I used to build a wheel file for a basic Python project using "uv-build".

---

I've used `cargo update -p mio`. This mio release is 2-3 months old.

---

_Renamed from "Update mio to 1.1.0" to "Update mio to 1.1.0 (for Cygwin)" by @lazka on 2026-01-01 11:02_

---

_@charliermarsh approved on 2026-01-01 13:24_

---

_Merged by @charliermarsh on 2026-01-01 13:25_

---

_Closed by @charliermarsh on 2026-01-01 13:25_

---

_Label `dependencies` added by @charliermarsh on 2026-01-01 13:25_

---

_Comment by @lazka on 2026-01-01 13:54_

Thanks!

---

_Label `internal` added by @konstin on 2026-01-06 09:51_

---
