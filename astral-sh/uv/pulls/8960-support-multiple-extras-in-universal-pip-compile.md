```yaml
number: 8960
title: Support multiple extras in universal pip compile output
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/platform-extras
created_at: 2024-11-08T23:03:40Z
updated_at: 2024-11-08T23:22:30Z
url: https://github.com/astral-sh/uv/pull/8960
synced_at: 2026-01-12T16:08:34Z
```

# Support multiple extras in universal pip compile output

---

_@charliermarsh_

## Summary

We were making some incorrect assumptions in the extra-merging code for universal `pip compile`. This PR corrects those assumptions and adds a bunch of additional tests.

Closes https://github.com/astral-sh/uv/issues/8915.


---

_Label `bug` added by @charliermarsh on 2024-11-08 23:03_

---

_@charliermarsh reviewed on 2024-11-08 23:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/display.rs`:405 on 2024-11-08 23:04_

I could make this a trait to DRY up the above but... I don't know, it seems like a lot of complexity.

---

_Merged by @charliermarsh on 2024-11-08 23:22_

---

_Closed by @charliermarsh on 2024-11-08 23:22_

---

_Branch deleted on 2024-11-08 23:22_

---
