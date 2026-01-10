---
number: 11016
title: "`interpreter_request_from_str` fails in subdirectory"
type: issue
state: open
author: konstin
labels:
  - internal
assignees: []
created_at: 2025-01-28T10:27:30Z
updated_at: 2025-01-28T14:31:21Z
url: https://github.com/astral-sh/uv/issues/11016
synced_at: 2026-01-10T01:25:00Z
---

# `interpreter_request_from_str` fails in subdirectory

---

_Issue opened by @konstin on 2025-01-28 10:27_

In my terminal (ubuntu 24.04), `interpreter_request_from_str` fails when run in a subdirectory:

```
$ cargo nextest run -E "test(discovery::tests::interpreter_request_from_str)"
[...]
thread 'discovery::tests::interpreter_request_from_str' panicked at crates/uv-python/src/discovery.rs:2712:9:
assertion `left == right` failed: A string with a file system separator is treated as a file
  left: Directory("/home/konsti/projects/uv/debug/./foo")
 right: File("./foo")
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[...]
```

The test output seems to depend on the current directory.

---

_Label `internal` added by @konstin on 2025-01-28 10:27_

---

_Comment by @zanieb on 2025-01-28 14:21_

Does the `foo` directory exist?

---

_Comment by @zanieb on 2025-01-28 14:22_

Maybe this is a bug with

https://github.com/astral-sh/uv/blob/db4ab9dc8aecd9c4deaffd6036304aae70d93791/crates/uv-python/src/discovery.rs#L1361-L1376

---

_Comment by @konstin on 2025-01-28 14:27_

Yes, I had a `foo` subdirectory.

---

_Comment by @zanieb on 2025-01-28 14:29_

Ah okay. I don't think we isolate these tests from state by default.

---

_Comment by @zanieb on 2025-01-28 14:31_

I don't see how we could without moving the test into a subprocess, unfortunately.

---
