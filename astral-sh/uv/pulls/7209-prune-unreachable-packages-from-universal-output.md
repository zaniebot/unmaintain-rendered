```yaml
number: 7209
title: "Prune unreachable packages from `--universal` output"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prune
created_at: 2024-09-09T01:38:24Z
updated_at: 2024-09-09T13:20:27Z
url: https://github.com/astral-sh/uv/pull/7209
synced_at: 2026-01-12T16:07:44Z
```

# Prune unreachable packages from `--universal` output

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/7196.


---

_Label `bug` added by @charliermarsh on 2024-09-09 01:38_

---

_Review requested from @konstin by @charliermarsh on 2024-09-09 01:38_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:230 on 2024-09-09 03:44_

neat, didn't realize we can discard the original markers here already

---

_@konstin approved on 2024-09-09 03:45_

---

_Merged by @charliermarsh on 2024-09-09 13:20_

---

_Closed by @charliermarsh on 2024-09-09 13:20_

---

_Branch deleted on 2024-09-09 13:20_

---
