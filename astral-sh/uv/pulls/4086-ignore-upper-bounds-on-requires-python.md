```yaml
number: 4086
title: "Ignore upper-bounds on `Requires-Python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/requires-python-cap
created_at: 2024-06-06T03:50:02Z
updated_at: 2024-06-06T17:52:58Z
url: https://github.com/astral-sh/uv/pull/4086
synced_at: 2026-01-10T13:54:02Z
```

# Ignore upper-bounds on `Requires-Python`

---

_Pull request opened by @charliermarsh on 2024-06-06 03:50_

## Summary

This PR modifies our `Requires-Python` handling to treat `Requires-Python` as a lower bound. There's extensive discussion around this in https://github.com/astral-sh/uv/issues/4022 and the references linked therein. I think it's an experiment worth trying. Even in my own small projects, I'm running into issues whereby I'm being "forced" to add a `<4` upper bound to my `Requires-Python` due to these caps.

Separately, we should explore adding a mechanism that's distinct from `Requires-Python` to enable users to declare a supported range for locking.

Closes https://github.com/astral-sh/uv/issues/4022.


---

_Marked ready for review by @charliermarsh on 2024-06-06 03:50_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-06 03:50_

---

_Review requested from @konstin by @charliermarsh on 2024-06-06 03:50_

---

_Label `preview` added by @charliermarsh on 2024-06-06 03:53_

---

_@konstin approved on 2024-06-06 08:03_

We need to document this in the user docs, partially respecting `requires-python` would be a headache to figure out otherwise.

---

_@zanieb approved on 2024-06-06 11:43_

---

_Merged by @charliermarsh on 2024-06-06 17:52_

---

_Closed by @charliermarsh on 2024-06-06 17:52_

---

_Branch deleted on 2024-06-06 17:52_

---
