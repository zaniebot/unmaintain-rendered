```yaml
number: 8730
title: Avoid showing dependency group annotations on workspace members in tree
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/de
created_at: 2024-10-31T17:35:16Z
updated_at: 2024-10-31T20:09:38Z
url: https://github.com/astral-sh/uv/pull/8730
synced_at: 2026-01-10T12:08:44Z
```

# Avoid showing dependency group annotations on workspace members in tree

---

_Pull request opened by @charliermarsh on 2024-10-31 17:35_

## Summary

By default, `uv tree` shows the full workspace, not _just_ the root. If the root depended on a workspace member as a dev dependency, then we'd still show it as `(group: dev)` in `uv tree` even if you passed `--no-dev`, because we weren't filtering the edges in the right place.

This is still somewhat confusing, because if `root` depends on workspace member `child` as a dev dependency, `uv tree --no-dev` still shows both.

Closes https://github.com/astral-sh/uv/issues/8719.


---

_Label `bug` added by @charliermarsh on 2024-10-31 17:38_

---

_@charliermarsh reviewed on 2024-10-31 19:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_tree.rs`:1 on 2024-10-31 19:47_

```suggestion
#![cfg(not(windows))]
```

---

_Merged by @charliermarsh on 2024-10-31 20:09_

---

_Closed by @charliermarsh on 2024-10-31 20:09_

---

_Branch deleted on 2024-10-31 20:09_

---
