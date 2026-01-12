```yaml
number: 8264
title: "Show hint in resolution failure on `Forbidden` (`403`) or `Unauthorized` (`401`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/index-warning
created_at: 2024-10-16T17:21:48Z
updated_at: 2024-10-16T19:02:21Z
url: https://github.com/astral-sh/uv/pull/8264
synced_at: 2026-01-12T16:08:14Z
```

# Show hint in resolution failure on `Forbidden` (`403`) or `Unauthorized` (`401`)

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/8167.


---

_Label `error messages` added by @charliermarsh on 2024-10-16 17:21_

---

_@zanieb approved on 2024-10-16 17:25_

Love it.

---

_Comment by @charliermarsh on 2024-10-16 17:26_

This was really bothering me haha.

---

_Merged by @charliermarsh on 2024-10-16 17:34_

---

_Closed by @charliermarsh on 2024-10-16 17:34_

---

_Branch deleted on 2024-10-16 17:34_

---

_@paveldikov reviewed on 2024-10-16 19:02_

---

_Review comment by @paveldikov on `crates/uv-resolver/src/pubgrub/report.rs`:1261 on 2024-10-16 19:02_

this is the same as 401, but imo should not be

maybe:
* Index URL could not be queried due to a lack of permissions

or IMO better (because 403s can be, and often are, resource specific)
* Package download for URL (...) was denied by the index.

(do you have access to whole resource URL in this context?)

---
