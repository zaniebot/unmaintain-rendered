```yaml
number: 3055
title: Add compatibility arguments for pip list
type: pull_request
state: merged
author: amv213
labels:
  - compatibility
assignees: []
merged: true
base: main
head: amv213/pip-list-outdated
created_at: 2024-04-16T07:36:44Z
updated_at: 2024-04-16T13:35:57Z
url: https://github.com/astral-sh/uv/pull/3055
synced_at: 2026-01-10T14:43:31Z
```

# Add compatibility arguments for pip list

---

_Pull request opened by @amv213 on 2024-04-16 07:36_

Hello! This is my first PR so do not hesitate to let me know if anything should be done differently 🙌🏽  

## Summary

This PR starts adding useful error messages and warnings when people pass redundant or unsupported arguments to `pip list`. 

For now, I've just covered `pip list --outdated`, which is currently unsupported.

Closes https://github.com/astral-sh/uv/issues/2948


---

_@charliermarsh approved on 2024-04-16 13:30_

Thanks, this makes sense to me. 

---

_Label `compatibility` added by @charliermarsh on 2024-04-16 13:30_

---

_Merged by @charliermarsh on 2024-04-16 13:31_

---

_Closed by @charliermarsh on 2024-04-16 13:31_

---

_Branch deleted on 2024-04-16 13:35_

---
