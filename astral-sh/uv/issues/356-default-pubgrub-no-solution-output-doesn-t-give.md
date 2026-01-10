---
number: 356
title: "Default pubgrub no solution output doesn't give any information"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-07T13:33:22Z
updated_at: 2023-11-08T14:57:28Z
url: https://github.com/astral-sh/uv/issues/356
synced_at: 2026-01-10T01:23:04Z
---

# Default pubgrub no solution output doesn't give any information

---

_Issue opened by @konstin on 2023-11-07 13:33_

In place where we don't manually format pubgrub errors, such as resolving build environment, we don't get any context on the resolution failure:

```console
$ cargo run --bin puffin-dev -q -- resolve-cli tensorflow-cpu-aws
puffin-dev failed
  Caused by: No solution found when resolving build dependencies for source distribution build
  Caused by: No solution
```

The error message should use our `ResolutionFailureReporter` instead of discarding the derivation tree:

https://github.com/astral-sh/puffin/blob/2ba85bf80e9b8d6ef75d5b8499a1004a904adb10/vendor/pubgrub/src/error.rs#L14-L16

One option would be to always wrap pubgrub errors into a custom error type

---

_Label `bug` added by @charliermarsh on 2023-11-08 05:02_

---

_Referenced in [astral-sh/uv#365](../../astral-sh/uv/pulls/365.md) on 2023-11-08 05:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-08 05:03_

---

_Closed by @charliermarsh on 2023-11-08 14:57_

---
