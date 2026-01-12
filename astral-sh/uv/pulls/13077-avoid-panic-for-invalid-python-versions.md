```yaml
number: 13077
title: Avoid panic for invalid Python versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/pyv
created_at: 2025-04-23T22:09:35Z
updated_at: 2025-04-24T12:00:12Z
url: https://github.com/astral-sh/uv/pull/13077
synced_at: 2026-01-12T16:10:32Z
```

# Avoid panic for invalid Python versions

---

_@charliermarsh_

## Summary

We unwrap these further on, so we should validate them ahead of time.

Closes https://github.com/astral-sh/uv/issues/13075.


---

_Label `bug` added by @charliermarsh on 2025-04-23 22:09_

---

_Label `error messages` added by @charliermarsh on 2025-04-23 22:09_

---

_Merged by @charliermarsh on 2025-04-23 22:23_

---

_Closed by @charliermarsh on 2025-04-23 22:23_

---

_Branch deleted on 2025-04-23 22:23_

---

_@konstin reviewed on 2025-04-24 08:30_

Can we store the parsed u8's instead?

---

_Comment by @charliermarsh on 2025-04-24 12:00_

I considered that but decided not to denormalize since it’s just a field access on the underlying struct. I think either approach would be totally fine.

---
