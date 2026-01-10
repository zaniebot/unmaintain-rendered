```yaml
number: 14863
title: "Respect `--with` versions over base environment versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/with
created_at: 2025-07-24T01:43:01Z
updated_at: 2025-07-24T02:00:04Z
url: https://github.com/astral-sh/uv/pull/14863
synced_at: 2026-01-10T06:53:02Z
```

# Respect `--with` versions over base environment versions

---

_Pull request opened by @charliermarsh on 2025-07-24 01:43_

## Summary

This fixes a regression from https://github.com/astral-sh/uv/pull/14447 that we seemingly didn't have test coverage for. Specifically, if you have a version of a package in your project, and then install a different version with `--with`, the environment should import the `--with` version.

Closes #14860.


---

_Label `bug` added by @charliermarsh on 2025-07-24 01:43_

---

_@charliermarsh reviewed on 2025-07-24 01:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:1073 on 2025-07-24 01:43_

TIL that earlier `addsitedir` calls get priority over later.

---

_Marked ready for review by @charliermarsh on 2025-07-24 01:43_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-24 01:43_

---

_@zanieb approved on 2025-07-24 01:49_

---

_Merged by @charliermarsh on 2025-07-24 02:00_

---

_Closed by @charliermarsh on 2025-07-24 02:00_

---

_Branch deleted on 2025-07-24 02:00_

---
