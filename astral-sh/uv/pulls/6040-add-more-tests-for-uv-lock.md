```yaml
number: 6040
title: Add more tests for uv lock
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/tests
created_at: 2024-08-12T19:15:11Z
updated_at: 2024-08-12T19:56:04Z
url: https://github.com/astral-sh/uv/pull/6040
synced_at: 2026-01-12T16:07:10Z
```

# Add more tests for uv lock

---

_@charliermarsh_

## Summary

Misc. cases we're missing vis-a-vis pip install.

---

_Label `testing` added by @charliermarsh on 2024-08-12 19:15_

---

_@charliermarsh reviewed on 2024-08-12 19:16_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:16_

We may need to revisit this, I'm not sure.

---

_@zanieb reviewed on 2024-08-12 19:28_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:28_

We're writing the token to the lockfile?

---

_@charliermarsh reviewed on 2024-08-12 19:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:35_

Yeah. If we don't, how would the user be able to provide it in the future, like when installing?

---

_@charliermarsh reviewed on 2024-08-12 19:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:35_

It differs from registries in that way... With registries, there's a mechanism for providing credentials at install-time.

---

_@zanieb reviewed on 2024-08-12 19:38_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:38_

I think your Git credentials can be fetched from the system keychain (or whatever store Git is configured to use). It's tough when someone hands us credentials directly... but it seems bad to make it easy to commit them to VCS in a generated file.

---

_@charliermarsh reviewed on 2024-08-12 19:55_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:5330 on 2024-08-12 19:55_

Yeah I'm not sure what's right here. It feels like a footgun either way (e.g., if we omit, then `lock` followed by `sync` fails).

---

_Merged by @charliermarsh on 2024-08-12 19:56_

---

_Closed by @charliermarsh on 2024-08-12 19:56_

---

_Branch deleted on 2024-08-12 19:56_

---
