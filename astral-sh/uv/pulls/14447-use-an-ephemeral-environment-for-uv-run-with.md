```yaml
number: 14447
title: "Use an ephemeral environment for `uv run --with` invocations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: release/080
head: charlie/eph
created_at: 2025-07-03T19:36:20Z
updated_at: 2025-07-14T17:18:41Z
url: https://github.com/astral-sh/uv/pull/14447
synced_at: 2026-01-10T06:53:01Z
```

# Use an ephemeral environment for `uv run --with` invocations

---

_Pull request opened by @charliermarsh on 2025-07-03 19:36_

## Summary

This PR creates separation between the `--with` environment and the environment we actually run in, which in turn solves issues like https://github.com/astral-sh/uv/issues/12889 whereby two invocations share the same `--with` environment, causing them to collide by way of sharing an overlay.

Closes https://github.com/astral-sh/uv/issues/7643.


---

_Label `bug` added by @charliermarsh on 2025-07-03 19:36_

---

_Marked ready for review by @charliermarsh on 2025-07-03 20:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:81 on 2025-07-03 20:29_

We should retain this commentary in some form, I think.

---

_@zanieb reviewed on 2025-07-03 20:29_

---

_@zanieb reviewed on 2025-07-03 20:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/environment.rs`:98 on 2025-07-03 20:29_

And this todo is still valid

---

_Comment by @zanieb on 2025-07-03 20:31_

Nice, this makes sense to me. I'll give it a closer look on Monday.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/environment.rs`:81 on 2025-07-03 20:33_

Okay, will tweak. (I just did a clean revert of the file for simplicity.)

---

_@charliermarsh reviewed on 2025-07-03 20:33_

---

_@charliermarsh reviewed on 2025-07-03 21:31_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/environment.rs`:98 on 2025-07-03 21:31_

How / why would this happen?

---

_Comment by @tjni on 2025-07-09 15:56_

Thank you for digging this up and fixing it. I really appreciate it!

---

_@zanieb approved on 2025-07-11 19:02_

---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-07-14 17:00_

---

_Merged by @charliermarsh on 2025-07-14 17:18_

---

_Closed by @charliermarsh on 2025-07-14 17:18_

---

_Branch deleted on 2025-07-14 17:18_

---
