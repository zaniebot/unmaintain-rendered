```yaml
number: 6660
title: Avoid reusing state across tool upgrades
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/st
created_at: 2024-08-26T21:45:20Z
updated_at: 2024-08-26T22:09:07Z
url: https://github.com/astral-sh/uv/pull/6660
synced_at: 2026-01-12T16:07:28Z
```

# Avoid reusing state across tool upgrades

---

_@charliermarsh_

## Summary

Because tool upgrades can use different Python versions, we can't share state across them.

Closes https://github.com/astral-sh/uv/issues/6659.


---

_Label `bug` added by @charliermarsh on 2024-08-26 21:45_

---

_@zanieb approved on 2024-08-26 21:46_

---

_Comment by @zanieb on 2024-08-26 21:46_

Sneaky. Please add a test :)

---

_Comment by @charliermarsh on 2024-08-26 21:46_

Working on it!

---

_Merged by @charliermarsh on 2024-08-26 22:08_

---

_Closed by @charliermarsh on 2024-08-26 22:08_

---

_Branch deleted on 2024-08-26 22:08_

---

_Comment by @charliermarsh on 2024-08-26 22:09_

I deemed this too hard to test... The packages need to have a shared dependency, like `certifi`, since that's what causes us to downgrade.

---
