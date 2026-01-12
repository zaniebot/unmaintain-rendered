```yaml
number: 4707
title: "Narrow `requires-python` requirement in resolver forks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req-python
created_at: 2024-07-01T20:43:24Z
updated_at: 2024-07-02T12:23:40Z
url: https://github.com/astral-sh/uv/pull/4707
synced_at: 2026-01-12T16:06:24Z
```

# Narrow `requires-python` requirement in resolver forks

---

_@charliermarsh_

## Summary

Given:

```text
numpy >=1.26 ; python_version >= '3.9'
numpy <1.26 ; python_version < '3.9'
```

When resolving for Python 3.8, we need to narrow the `requires-python` requirement in the top branch of the fork, because `numpy >=1.26` all require Python 3.9 or later -- but we know (in that branch) that we only need to _solve_ for Python 3.9 or later.

Closes https://github.com/astral-sh/uv/issues/4669.


---

_Label `bug` added by @charliermarsh on 2024-07-01 20:43_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-01 20:44_

---

_Review requested from @konstin by @charliermarsh on 2024-07-01 20:44_

---

_@konstin approved on 2024-07-02 10:56_

---

_Merged by @charliermarsh on 2024-07-02 12:23_

---

_Closed by @charliermarsh on 2024-07-02 12:23_

---

_Branch deleted on 2024-07-02 12:23_

---
