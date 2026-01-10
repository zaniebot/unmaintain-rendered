```yaml
number: 2573
title: Avoid repeating paths of workspace packages
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/avoid-repeating-paths
created_at: 2024-03-20T19:44:00Z
updated_at: 2024-03-20T21:16:10Z
url: https://github.com/astral-sh/uv/pull/2573
synced_at: 2026-01-10T14:49:08Z
```

# Avoid repeating paths of workspace packages

---

_Pull request opened by @konstin on 2024-03-20 19:44_

Scott schafer got me the idea: We can avoid repeating the path for workspaces dependencies everywhere if we declare them in the virtual package once and treat them as workspace dependencies from there on.

---

_Label `internal` added by @konstin on 2024-03-20 19:44_

---

_Review requested from @MichaReiser by @konstin on 2024-03-20 19:44_

---

_@charliermarsh approved on 2024-03-20 19:47_

---

_Merged by @charliermarsh on 2024-03-20 20:16_

---

_Closed by @charliermarsh on 2024-03-20 20:16_

---

_Branch deleted on 2024-03-20 20:16_

---

_Comment by @zanieb on 2024-03-20 20:52_

This looks to have introduced a warning

```

warning: /Users/mz/workspace/uv/crates/uv/Cargo.toml: `default-features` is ignored for install-wheel-rs, since `default-features` was true for `workspace.dependencies.install-wheel-rs`, this could become a hard error in the future
warning: /Users/mz/workspace/uv/crates/uv-installer/Cargo.toml: `default-features` is ignored for install-wheel-rs, since `default-features` was true for `workspace.dependencies.install-wheel-rs`, this could become a hard error in the future
```

---

_Comment by @konstin on 2024-03-20 21:16_

Fixed in https://github.com/astral-sh/uv/pull/2576, i didn't expect this to get merged this fast.

---
