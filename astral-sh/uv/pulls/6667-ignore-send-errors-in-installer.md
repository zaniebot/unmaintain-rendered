```yaml
number: 6667
title: Ignore send errors in installer
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/send
created_at: 2024-08-27T02:16:05Z
updated_at: 2024-08-27T12:59:19Z
url: https://github.com/astral-sh/uv/pull/6667
synced_at: 2026-01-12T16:07:28Z
```

# Ignore send errors in installer

---

_@charliermarsh_

## Summary

Similar to https://github.com/astral-sh/uv/pull/6182.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 02:16_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-27 02:16_

---

_Label `bug` added by @charliermarsh on 2024-08-27 02:16_

---

_Comment by @ibraheemdev on 2024-08-27 02:59_

This one is a little different because the outer thread immediately calls `.await` on the receiver. The only way the inner task can fail is if the outer was cancelled.. which is possible, so this change is probably good, but it might also be worth investigating why the outer task is getting cancelled. This can happen if it was spawned with tokio::spawn or used in a combinator like select!, but I don't *think* we do any of that (off the top of my head).

---

_Review comment by @ibraheemdev on `crates/uv-installer/src/installer.rs`:98 on 2024-08-27 03:00_

```suggestion
            // This may fail if the main task was cancelled.
```

---

_@ibraheemdev approved on 2024-08-27 03:00_

---

_Merged by @charliermarsh on 2024-08-27 12:59_

---

_Closed by @charliermarsh on 2024-08-27 12:59_

---

_Branch deleted on 2024-08-27 12:59_

---
