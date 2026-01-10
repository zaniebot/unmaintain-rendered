---
number: 1846
title: "panic: Every package should have metadata: NameVersion(PackageName(\"coverage\"), \"6.4\")"
type: issue
state: closed
author: amartani
labels:
  - bug
assignees: []
created_at: 2024-02-22T02:09:50Z
updated_at: 2024-02-22T16:44:47Z
url: https://github.com/astral-sh/uv/issues/1846
synced_at: 2026-01-10T01:23:09Z
---

# panic: Every package should have metadata: NameVersion(PackageName("coverage"), "6.4")

---

_Issue opened by @amartani on 2024-02-22 02:09_

I can't reproduce it, but my first attempt at `uv pip install -r requirements.txt` failed with the following error:

```
thread 'main' panicked at crates/uv-resolver/src/resolution.rs:136:29:
Every package should have metadata: NameVersion(PackageName("coverage"), "6.4")
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Re-running the exact same command again works, so this seems to be some transient issue.

Running uv version 0.1.6 in Linux arm64 (docker in a M2 Macbook), pointing to an AWS CodeArtifact repository.

---

_Comment by @charliermarsh on 2024-02-22 02:11_

This may be fixed in the upcoming release.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 02:16_

---

_Label `bug` added by @charliermarsh on 2024-02-22 02:16_

---

_Comment by @johanez on 2024-02-22 09:53_

I got the same error with `ibis-framework[potsgres]` not with `ibis-framework`

---

_Comment by @charliermarsh on 2024-02-22 12:45_

Thanks. Is this with v0.1.7?

---

_Comment by @charliermarsh on 2024-02-22 16:44_

@johanez - I believe that was fixed in the latest version (`potsgres` is a typo of `postgres`, and the error here was in reporting invalid extras).

---

_Closed by @charliermarsh on 2024-02-22 16:44_

---
