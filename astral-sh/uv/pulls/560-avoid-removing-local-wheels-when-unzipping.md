```yaml
number: 560
title: Avoid removing local wheels when unzipping
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/separate
created_at: 2023-12-05T03:43:08Z
updated_at: 2023-12-05T17:50:09Z
url: https://github.com/astral-sh/uv/pull/560
synced_at: 2026-01-12T16:04:02Z
```

# Avoid removing local wheels when unzipping

---

_@charliermarsh_

## Summary

When installing a local wheel, we need to avoid removing the zipped wheel (since it lives outside of the cache), _and_ need to ensure that we unzip the wheel into the cache (rather than replacing the zipped wheel, which may even live outside of the project).

Closes https://github.com/astral-sh/puffin/issues/553.


---

_Label `bug` added by @charliermarsh on 2023-12-05 03:43_

---

_@charliermarsh reviewed on 2023-12-05 03:43_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:56 on 2023-12-05 03:43_

@konstin - This will remove the zipped wheel when appropriate by way of it colliding with `download.target()`. Let me know if you'd prefer something more explicit... E.g., check the kind of distribution, and then remove `download.path()`.

---

_Review requested from @konstin by @charliermarsh on 2023-12-05 03:49_

---

_Comment by @charliermarsh on 2023-12-05 17:45_

We agreed to merge this synchronously.

---

_Merged by @charliermarsh on 2023-12-05 17:50_

---

_Closed by @charliermarsh on 2023-12-05 17:50_

---

_Branch deleted on 2023-12-05 17:50_

---
