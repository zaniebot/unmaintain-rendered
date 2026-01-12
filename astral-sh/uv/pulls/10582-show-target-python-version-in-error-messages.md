```yaml
number: 10582
title: Show target Python version in error messages
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/show-tag
created_at: 2025-01-14T02:57:28Z
updated_at: 2025-01-15T20:08:40Z
url: https://github.com/astral-sh/uv/pull/10582
synced_at: 2026-01-12T16:09:22Z
```

# Show target Python version in error messages

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/pull/10527#discussion_r1913593405


---

_Label `error messages` added by @charliermarsh on 2025-01-14 02:57_

---

_@charliermarsh reviewed on 2025-01-14 02:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install_scenarios.rs`:4180 on 2025-01-14 02:58_

I considered doing the same for platform, but I'm not sure what to show exactly... I could say "Linux", but it might be a glibc incompatibility? If I say macOS, do I include the version, or the architecture?

---

_Review requested from @zanieb by @charliermarsh on 2025-01-14 03:51_

---

_@charliermarsh reviewed on 2025-01-14 03:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install_scenarios.rs`:4180 on 2025-01-14 03:51_

Separately, I don't love this message ("While solving for..."). Would love help.

---

_@zanieb reviewed on 2025-01-15 19:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install_scenarios.rs`:4180 on 2025-01-15 19:27_

Ah glad you don't love it either, that was my initial impression.

I'll help think of something.

---

_@zanieb approved on 2025-01-15 19:54_

I think I'll ship this and we can revisit if you want?

---

_@zanieb reviewed on 2025-01-15 19:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:13984 on 2025-01-15 19:57_

A concern here: is this valid in universal resolution?

---

_@zanieb reviewed on 2025-01-15 19:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:6761 on 2025-01-15 19:59_

This change for consistency with the rest of the error report.

Interesting we say "Python version tag" specifically above!

---

_Merged by @zanieb on 2025-01-15 20:08_

---

_Closed by @zanieb on 2025-01-15 20:08_

---

_Branch deleted on 2025-01-15 20:08_

---
