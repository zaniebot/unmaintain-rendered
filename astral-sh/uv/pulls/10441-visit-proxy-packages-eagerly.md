```yaml
number: 10441
title: Visit proxy packages eagerly
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prox
created_at: 2025-01-09T18:44:01Z
updated_at: 2025-01-21T22:28:37Z
url: https://github.com/astral-sh/uv/pull/10441
synced_at: 2026-01-12T16:09:17Z
```

# Visit proxy packages eagerly

---

_@charliermarsh_

## Summary

The issue here is that we add `urllib3{python_full_version >= '3.8'}` as a dependency, then `requests{python_full_version >= '3.8'}`, which adds `urllib3`, but at that point, we haven't expanded `urllib3{python_full_version >= '3.8'}`, so we "lose" the singleton constraint. The solution is to ensure that we visit proxies eagerly, so that we accumulate constraints as early as possible.

Closes https://github.com/astral-sh/uv/issues/10425.


---

_Review requested from @konstin by @charliermarsh on 2025-01-09 18:44_

---

_Label `bug` added by @charliermarsh on 2025-01-09 18:44_

---

_Marked ready for review by @charliermarsh on 2025-01-09 18:44_

---

_@charliermarsh reviewed on 2025-01-09 18:44_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:85 on 2025-01-09 18:44_

I'll fix this separately, but this fix alone does _not_ resolve the bug in the linked issue.

---

_Comment by @charliermarsh on 2025-01-09 19:04_

The error message changes seem... ok. The transformers ecosystem change is surprising.

---

_Comment by @charliermarsh on 2025-01-09 19:10_

Ok, think I fixed that regression.

---

_Comment by @charliermarsh on 2025-01-09 19:14_

I have a hunch that this will actually improve performance and lead to better resolutions.

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:230 on 2025-01-09 20:12_

Can you expand this by saying something like:

> We process these first since each proxy package expands into two regular `PubGrubPackage` packages, which give us additional constraints, while not changing the priorities since the materialized packages are of the same package name.

It took me a while to get why this was safe to do.

---

_@konstin approved on 2025-01-09 20:13_

---

_@zanieb approved on 2025-01-09 20:31_

---

_Merged by @charliermarsh on 2025-01-09 20:37_

---

_Closed by @charliermarsh on 2025-01-09 20:37_

---

_Branch deleted on 2025-01-09 20:37_

---
