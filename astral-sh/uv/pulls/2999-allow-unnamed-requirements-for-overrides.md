```yaml
number: 2999
title: Allow unnamed requirements for overrides
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/override
created_at: 2024-04-11T20:17:26Z
updated_at: 2024-04-11T21:19:12Z
url: https://github.com/astral-sh/uv/pull/2999
synced_at: 2026-01-12T16:05:21Z
```

# Allow unnamed requirements for overrides

---

_@charliermarsh_

## Summary

This PR lifts a constraint by allowing unnamed requirements in `overrides.txt` files.

---

_Comment by @charliermarsh on 2024-04-11 20:45_

Need tests.

---

_Marked ready for review by @charliermarsh on 2024-04-11 21:06_

---

_Label `enhancement` added by @charliermarsh on 2024-04-11 21:06_

---

_@charliermarsh reviewed on 2024-04-11 21:07_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:163 on 2024-04-11 21:07_

I realized that `satisfies` doesn't respect overrides at all, and it's actually difficult to support in our current model.

---

_Merged by @charliermarsh on 2024-04-11 21:19_

---

_Closed by @charliermarsh on 2024-04-11 21:19_

---

_Branch deleted on 2024-04-11 21:19_

---
