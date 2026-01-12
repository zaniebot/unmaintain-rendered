```yaml
number: 11254
title: Always use base Python discovery logic for cached environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/sym
created_at: 2025-02-05T19:11:05Z
updated_at: 2025-02-05T20:47:58Z
url: https://github.com/astral-sh/uv/pull/11254
synced_at: 2026-01-12T16:09:45Z
```

# Always use base Python discovery logic for cached environments

---

_@charliermarsh_

## Summary

This is attempting to solve the same problem surfaced in #11208 and #11209. However, those PRs only worked for our own managed Pythons. In Gentoo, for example, they disable the managed Pythons, which led to failures in the test suite, because the "base Python" returned after creating a virtual environment would differ from the "base Python" that you get after _querying_ an existing virtual environment.

The fix here is to apply our same base Python normalization and discovery logic, to non-standalone / non-managed Pythons. We continue to use `sys._base_executable` for such Pythons when creating the virtualenv, but when _caching_, we perform this second discovery step.

Closes https://github.com/astral-sh/uv/issues/11237.


---

_Label `bug` added by @charliermarsh on 2025-02-05 19:11_

---

_Marked ready for review by @charliermarsh on 2025-02-05 19:11_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-05 19:13_

---

_Comment by @charliermarsh on 2025-02-05 19:13_

\cc @mgorny


---

_@zanieb approved on 2025-02-05 19:14_

Some commentary on how `find_base_python` and `to_base_python` differ would be nice. It's not obvious.

---

_Comment by @charliermarsh on 2025-02-05 19:45_

There's a note in the rustdoc for each, but I'll expand it!

---

_Comment by @zanieb on 2025-02-05 19:53_

Yeah I read that note, but am looking for a reference to the other / an explanation of when you would want one vs the other.

---

_Merged by @charliermarsh on 2025-02-05 20:47_

---

_Closed by @charliermarsh on 2025-02-05 20:47_

---

_Branch deleted on 2025-02-05 20:47_

---
