```yaml
number: 2809
title: "Upgrade `rs-async-zip` to support data descriptors"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/async-zip
created_at: 2024-04-03T15:06:49Z
updated_at: 2024-04-04T01:31:41Z
url: https://github.com/astral-sh/uv/pull/2809
synced_at: 2026-01-10T14:49:08Z
```

# Upgrade `rs-async-zip` to support data descriptors

---

_Pull request opened by @charliermarsh on 2024-04-03 15:06_

## Summary

Upgrading `rs-async-zip` enables us to support data descriptors in streaming. This both greatly improves performance for indexes that use data descriptors _and_ ensures that we support them in a few other places (e.g., zipped source distributions created in Finder).

Closes #2808.


---

_Comment by @charliermarsh on 2024-04-03 22:09_

I've fixed all the perf regressions (and now see improvements, and significant improvements for files that use data descriptors), but I'm now seeing a very rare and sporadic "end of file" failure in some cases for files that contain data descriptors (https://github.com/Majored/rs-async-zip/issues/122#issuecomment-2035686451).


---

_Marked ready for review by @charliermarsh on 2024-04-04 01:26_

---

_Merged by @charliermarsh on 2024-04-04 01:31_

---

_Closed by @charliermarsh on 2024-04-04 01:31_

---

_Branch deleted on 2024-04-04 01:31_

---
