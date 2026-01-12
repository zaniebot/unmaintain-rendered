```yaml
number: 13844
title: Bump version to 0.7.11
type: pull_request
state: merged
author: oconnor663
labels:
  - releases
assignees: []
merged: true
base: main
head: jack/release_0.7.11
created_at: 2025-06-04T17:06:31Z
updated_at: 2025-06-04T19:35:57Z
url: https://github.com/astral-sh/uv/pull/13844
synced_at: 2026-01-12T16:10:53Z
```

# Bump version to 0.7.11

---

_@oconnor663_

_No description provided._

---

_@zanieb reviewed on 2025-06-04 17:09_

---

_Review comment by @zanieb on `CHANGELOG.md`:23 on 2025-06-04 17:09_

We generally move everything out of "Other changes", this is an "Enhancement"

---

_Review comment by @zanieb on `CHANGELOG.md`:14 on 2025-06-04 17:10_

Maybe something like...
```suggestion
- Downgrade `reqwest` and `hyper-util` to resolve connection reset errors over ipv6 ([#13835](https://github.com/astral-sh/uv/pull/13835))
```

---

_@zanieb reviewed on 2025-06-04 17:10_

---

_@zanieb approved on 2025-06-04 17:10_

---

_Label `releases` added by @zanieb on 2025-06-04 17:10_

---

_Comment by @oconnor663 on 2025-06-04 17:23_

This PR will wait for:

- https://github.com/astral-sh/python-build-standalone/actions/runs/15448469947
- https://github.com/astral-sh/python-build-standalone/actions/runs/15448469949
- https://github.com/astral-sh/python-build-standalone/actions/runs/15448469953

---

_Merged by @oconnor663 on 2025-06-04 19:35_

---

_Closed by @oconnor663 on 2025-06-04 19:35_

---

_Branch deleted on 2025-06-04 19:35_

---
