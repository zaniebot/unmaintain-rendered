```yaml
number: 1982
title: Future-proof pip entrypoints special case
type: pull_request
state: merged
author: konstin
labels:
  - compatibility
assignees: []
merged: true
base: main
head: konsti/update-pip-special-case
created_at: 2024-02-26T13:18:47Z
updated_at: 2024-03-01T09:05:51Z
url: https://github.com/astral-sh/uv/pull/1982
synced_at: 2026-01-12T16:04:49Z
```

# Future-proof pip entrypoints special case

---

_@konstin_

Update #1918 to handle https://github.com/pypa/pip/pull/12536, where pip removed their python minor entrypoint. The pip test is semi-functional since it builds pip from source instead of using a wheel with the wrong entrypoint, we have to update it when this pip version has a release.

Closes #1593.


---

_Label `compatibility` added by @konstin on 2024-02-26 13:18_

---

_@konstin reviewed on 2024-02-26 13:19_

---

_Review comment by @konstin on `crates/uv/tests/pip_sync.rs`:2780 on 2024-02-26 13:19_

I'd skip the output here since it's different for the two different versions

---

_Merged by @konstin on 2024-03-01 09:05_

---

_Closed by @konstin on 2024-03-01 09:05_

---

_Branch deleted on 2024-03-01 09:05_

---
