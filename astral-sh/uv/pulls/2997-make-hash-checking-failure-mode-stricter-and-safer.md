```yaml
number: 2997
title: Make hash-checking failure mode stricter and safer
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/validate
created_at: 2024-04-11T17:35:24Z
updated_at: 2024-04-11T17:53:35Z
url: https://github.com/astral-sh/uv/pull/2997
synced_at: 2026-01-12T16:05:21Z
```

# Make hash-checking failure mode stricter and safer

---

_@charliermarsh_

## Summary

If there are no hashes for a given package, we now return `Validate(&[])` so that the policy is impossible to satisfy. Previously, we returned `None`, which is always satisfied.

We don't really ever expect to hit this, because we detect this case in the resolver and raise a different error. But if we have a bug somewhere, it's better to fail with an error than silently let the package through.


---

_Marked ready for review by @charliermarsh on 2024-04-11 17:35_

---

_Label `enhancement` added by @charliermarsh on 2024-04-11 17:35_

---

_Label `enhancement` removed by @charliermarsh on 2024-04-11 17:41_

---

_Label `error messages` added by @charliermarsh on 2024-04-11 17:41_

---

_Merged by @charliermarsh on 2024-04-11 17:53_

---

_Closed by @charliermarsh on 2024-04-11 17:53_

---

_Branch deleted on 2024-04-11 17:53_

---
