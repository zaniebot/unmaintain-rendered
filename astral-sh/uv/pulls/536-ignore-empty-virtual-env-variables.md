```yaml
number: 536
title: "Ignore empty `VIRTUAL_ENV` variables"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2023-12-04T04:47:53Z
updated_at: 2023-12-04T08:54:20Z
url: https://github.com/astral-sh/uv/pull/536
synced_at: 2026-01-12T16:04:01Z
```

# Ignore empty `VIRTUAL_ENV` variables

---

_@charliermarsh_

I'm not sure how my interpreter gets into this state, but it's certainly wrong to respect these.

---

_Label `bug` added by @charliermarsh on 2023-12-04 04:50_

---

_Merged by @charliermarsh on 2023-12-04 04:53_

---

_Closed by @charliermarsh on 2023-12-04 04:53_

---

_Branch deleted on 2023-12-04 04:53_

---

_Comment by @konstin on 2023-12-04 08:54_

That's an odd one, i'd be interested if you find a reproduction, no one has reported this to maturin so far.

---
