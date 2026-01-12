```yaml
number: 9594
title: Respect path dependencies within Git dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/path
created_at: 2024-12-03T03:40:00Z
updated_at: 2024-12-03T05:13:31Z
url: https://github.com/astral-sh/uv/pull/9594
synced_at: 2026-01-12T16:08:53Z
```

# Respect path dependencies within Git dependencies

---

_@charliermarsh_

## Summary

If a Git repository uses a `path` dependency (rather than a `workspace`), we need to expand the path to make it relative to the Git root.

Closes https://github.com/astral-sh/uv/issues/9516.


---

_Review requested from @konstin by @charliermarsh on 2024-12-03 03:40_

---

_Label `bug` added by @charliermarsh on 2024-12-03 03:40_

---

_@charliermarsh reviewed on 2024-12-03 03:40_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:760 on 2024-12-03 03:40_

I'm not sure what to do here. We don't have a way to represent this? It's like if you had a Git repo that included a `.whl` file, and another package referred to it via a `path` dependency. We allow that locally, but we can't "translate" that to Git.

---

_Merged by @charliermarsh on 2024-12-03 05:13_

---

_Closed by @charliermarsh on 2024-12-03 05:13_

---

_Branch deleted on 2024-12-03 05:13_

---
