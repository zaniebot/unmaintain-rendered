```yaml
number: 1565
title: "LSP panic: assertion failed: old_memo.revisions.changed_at <= revisions.changed_atquery"
type: issue
state: open
author: correctmost
labels:
  - fatal
assignees: []
created_at: 2025-11-14T21:23:37Z
updated_at: 2025-12-31T15:35:20Z
url: https://github.com/astral-sh/ty/issues/1565
synced_at: 2026-01-10T01:56:40Z
```

# LSP panic: assertion failed: old_memo.revisions.changed_at <= revisions.changed_atquery

---

_Issue opened by @correctmost on 2025-11-14 21:23_

### Summary

I'm not sure what triggers this panic because I've only seen it twice over the course of a few weeks.  

Hopefully the log has some useful info:

```
2025-11-14 15:03:01.451648045  INFO Version: 0.0.1-alpha.26
2025-11-14 15:03:02.694769551  INFO Defaulting to python-platform `linux`
2025-11-14 15:03:02.702819518  INFO Python version: Python 3.13, platform: linux
2025-11-14 15:03:03.187841405  INFO Indexed 285 file(s) in 0.188s
2025-11-14 15:05:02.050690516  INFO Initializing the default project
2025-11-14 15:05:02.051145907  INFO Defaulting to python-platform `linux`
2025-11-14 15:05:02.058633824  INFO Python version: Python 3.13, platform: linux
2025-11-14 15:49:18.766983034  INFO Checking file `untitled:Untitled-1` took more than 100ms (222.307918ms)
2025-11-14 15:50:17.476954023  INFO Checking file `untitled:Untitled-1` took more than 100ms (163.922204ms)
2025-11-14 15:50:18.527326740  WARN Propagating panic for cycle head that panicked in an earlier execution in that revision
2025-11-14 15:50:18.530508190 ERROR An error occurred with request ID 5449: request handler panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_atquery stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

[Error - 3:50:18 PM] Request textDocument/diagnostic failed.
  Message: request handler panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/backdate.rs:42:17:
assertion failed: old_memo.revisions.changed_at <= revisions.changed_atquery stacktrace:


run with `RUST_BACKTRACE=1` environment variable to display a backtrace

  Code: -32603 
```

### Version

0.0.1-alpha.26

---

_Label `server` added by @carljm on 2025-11-14 21:46_

---

_Label `fatal` added by @carljm on 2025-11-14 21:46_

---

_Label `server` removed by @MichaReiser on 2025-11-14 22:54_

---

_Comment by @MichaReiser on 2025-11-16 07:33_

Thanks for the detailed report. I believe this has the same underlying issue as https://github.com/astral-sh/ty/issues/992

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:35_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-12-31 15:35_

---
