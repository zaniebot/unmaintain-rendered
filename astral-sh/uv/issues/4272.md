```yaml
number: 4272
title: "Marker normalization doesn't collapse non-pre-release ranges"
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
  - preview
assignees: []
created_at: 2024-06-12T14:15:46Z
updated_at: 2024-07-04T20:06:53Z
url: https://github.com/astral-sh/uv/issues/4272
synced_at: 2026-01-10T05:31:37Z
```

# Marker normalization doesn't collapse non-pre-release ranges

---

_Issue opened by @charliermarsh on 2024-06-12 14:15_

We don't recognize that `python_version < "3.7" or python_version >= "3.7"` evaluates to `true` because of our use of min versions to handle pre-releases.


---

_Label `bug` added by @charliermarsh on 2024-06-12 14:15_

---

_Label `preview` added by @charliermarsh on 2024-06-12 14:15_

---

_Comment by @charliermarsh on 2024-06-12 14:19_

Although, it is true that `3.7.0a0` does not match either of these markers.

---

_Label `bug` removed by @charliermarsh on 2024-06-12 14:24_

---

_Label `needs-decision` added by @charliermarsh on 2024-06-12 14:24_

---

_Comment by @charliermarsh on 2024-06-12 14:25_

What if we had a rule like... if none of the versions contain pre-release or post-release or dev release or local release, then we use different semantics and ignore those things?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-02 21:45_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---
