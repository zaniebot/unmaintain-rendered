```yaml
number: 3482
title: "Remove `Optional` from `with_origin` API"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/opt
created_at: 2024-05-09T05:25:54Z
updated_at: 2024-05-09T13:40:52Z
url: https://github.com/astral-sh/uv/pull/3482
synced_at: 2026-01-12T16:05:40Z
```

# Remove `Optional` from `with_origin` API

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-05-09 05:25_

---

_@zanieb approved on 2024-05-09 13:14_

Wondering why? I've seen this same pattern is used in things like `reqwest`. Isn't this the only way to unset the origin? Does that just not matter for this type?

---

_Comment by @charliermarsh on 2024-05-09 13:36_

Yeah for me:

- If the default is `None`...
- And all callers are providing a `Some`...
- And I can't see a concrete case in which a client would _want_ to unset...

Then I generally remove it to keep the API simpler, and also to make it impossible to unset.


---

_Merged by @charliermarsh on 2024-05-09 13:40_

---

_Closed by @charliermarsh on 2024-05-09 13:40_

---

_Branch deleted on 2024-05-09 13:40_

---
