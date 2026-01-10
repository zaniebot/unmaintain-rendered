```yaml
number: 3715
title: Panic with relative path beyond base directory
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-05-21T18:39:28Z
updated_at: 2024-05-21T19:59:01Z
url: https://github.com/astral-sh/uv/issues/3715
synced_at: 2026-01-10T05:31:37Z
```

# Panic with relative path beyond base directory

---

_Issue opened by @zanieb on 2024-05-21 18:39_

```
❯ echo "/../test" | uv pip compile -
thread 'main' panicked at crates/pep508-rs/src/verbatim_url.rs:81:42:
path is absolute: Custom { kind: InvalidInput, error: "cannot normalize a relative path beyond the base directory" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

We should throw a better error message here instead of panicking

---

_Label `error messages` added by @zanieb on 2024-05-21 18:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-21 18:56_

---

_Closed by @charliermarsh on 2024-05-21 19:59_

---
