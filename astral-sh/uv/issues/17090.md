```yaml
number: 17090
title: "`uv publish` can panic after a network failure"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-12-11T19:19:24Z
updated_at: 2025-12-12T17:02:50Z
url: https://github.com/astral-sh/uv/issues/17090
synced_at: 2026-01-10T03:11:35Z
```

# `uv publish` can panic after a network failure

---

_Issue opened by @konstin on 2025-12-11 19:19_

I can reliable get uv to panic by toggling network on and off during publishing a large wheel.

```
$ UV_HTTP_TIMEOUT=3 uv publish debug/dist/* --token ferris
Publishing 2 files to https://upload.pypi.org/legacy/
Uploading debug-0.0.4-py3-none-any.whl (854.7MiB)
warning: Transient failure while handling response for https://upload.pypi.org/legacy/; retrying after 0s...

thread 'main2' (501447) panicked at crates/uv/src/commands/reporters.rs:300:35:
no entry found for key
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

thread 'main2' (501447) panicked at crates/uv/src/commands/reporters.rs:221:38:
called `Result::unwrap()` on an `Err` value: PoisonError { .. }

thread 'main' (501446) panicked at /home/runner/work/uv/uv/crates/uv/src/lib.rs:2540:10:
Tokio executor failed, was there a panic?: Any { .. }
```

---

_Assigned to @konstin by @konstin on 2025-12-11 19:19_

---

_Label `bug` added by @konstin on 2025-12-11 19:19_

---

_Comment by @konstin on 2025-12-12 10:06_

This is an upstream bug with reqwest+rustls: https://github.com/seanmonstar/reqwest/issues/2884

---

_Closed by @konstin on 2025-12-12 17:02_

---
