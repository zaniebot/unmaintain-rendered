```yaml
number: 3711
title: Improve display of root package in range errors
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/err-root-range
created_at: 2024-05-21T17:53:27Z
updated_at: 2024-05-21T19:28:24Z
url: https://github.com/astral-sh/uv/pull/3711
synced_at: 2026-01-10T14:32:20Z
```

# Improve display of root package in range errors

---

_Pull request opened by @zanieb on 2024-05-21 17:53_

Instead of saying 

> we can conclude that you require==0a0.dev0 and pandas-stubs==2.0.3.230814 are incompatible.

we'll say

> we can conclude that your requirements and pandas-stubs==2.0.3.230814 are incompatible.

Closes #3710 

I'm not sure how to get unit test coverage for this, might look into that. Ideally we'd skip this branch entirely?

---

_Label `error messages` added by @zanieb on 2024-05-21 17:53_

---

_Review requested from @ibraheemdev by @zanieb on 2024-05-21 18:02_

---

_Marked ready for review by @zanieb on 2024-05-21 18:02_

---

_@ibraheemdev approved on 2024-05-21 18:06_

Makes sense to me. Could we add a packse scenario for this?

---

_Comment by @zanieb on 2024-05-21 18:24_

Last time I changed this in https://github.com/astral-sh/uv/pull/1871 I also noted we had no test coverage.. hm

This message looks like an improvement for the case in https://github.com/astral-sh/uv/issues/1855 though

---

_Comment by @zanieb on 2024-05-21 18:26_

I'm honestly not sure what the scenario is that prompts this. I'll try to think of one before merging.

---

_Comment by @zanieb on 2024-05-21 18:52_

Tracking further improvements in https://github.com/astral-sh/uv/issues/3716

---

_Merged by @zanieb on 2024-05-21 19:28_

---

_Closed by @zanieb on 2024-05-21 19:28_

---

_Branch deleted on 2024-05-21 19:28_

---
