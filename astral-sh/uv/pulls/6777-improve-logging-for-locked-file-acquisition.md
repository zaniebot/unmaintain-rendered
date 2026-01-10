```yaml
number: 6777
title: Improve logging for locked file acquisition
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/lock-debug
created_at: 2024-08-28T21:38:35Z
updated_at: 2024-09-03T18:14:02Z
url: https://github.com/astral-sh/uv/pull/6777
synced_at: 2026-01-10T12:53:34Z
```

# Improve logging for locked file acquisition

---

_Pull request opened by @zanieb on 2024-08-28 21:38_

Consistency etc. when debugging a deadlock 

---

_Label `tracing` added by @zanieb on 2024-08-28 21:38_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-28 21:59_

---

_Merged by @zanieb on 2024-08-29 20:35_

---

_Closed by @zanieb on 2024-08-29 20:35_

---

_Branch deleted on 2024-08-29 20:35_

---

_@charliermarsh reviewed on 2024-09-02 19:38_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:388 on 2024-09-02 19:38_

Should this be `trace!` like the above?

---

_@charliermarsh reviewed on 2024-09-02 19:38_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:349 on 2024-09-02 19:38_

Should this be `trace!` like the above?

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:388 on 2024-09-03 14:50_

Maybe since it comes after all the other output.

---

_@zanieb reviewed on 2024-09-03 14:50_

---

_@zanieb reviewed on 2024-09-03 14:51_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:349 on 2024-09-03 14:51_

I don't think so — the acquired lock message above is debug level. The trace logs are for the acquire lock error message and the check message.

---

_@charliermarsh reviewed on 2024-09-03 18:07_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:349 on 2024-09-03 18:07_

It still feels like a lot for `--verbose` IMO.

---

_@zanieb reviewed on 2024-09-03 18:14_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:349 on 2024-09-03 18:14_

That's fine with me I think, but we'll want to change both.

---
