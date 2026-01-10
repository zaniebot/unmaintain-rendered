```yaml
number: 4900
title: "Respect `requires-python` when prefetching"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2024-07-08T14:59:58Z
updated_at: 2024-07-08T16:32:10Z
url: https://github.com/astral-sh/uv/pull/4900
synced_at: 2026-01-10T13:42:52Z
```

# Respect `requires-python` when prefetching

---

_Pull request opened by @charliermarsh on 2024-07-08 14:59_

## Summary

This is fallout from https://github.com/astral-sh/uv/pull/4705. We need to respect `requires-python` in the prefetch code to avoid building unsupported distributions.

Closes https://github.com/astral-sh/uv/issues/4898.


---

_Label `bug` added by @charliermarsh on 2024-07-08 15:00_

---

_Label `resolver` added by @charliermarsh on 2024-07-08 15:00_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-08 15:01_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-08 15:01_

---

_Comment by @charliermarsh on 2024-07-08 15:01_

Will add test coverage.

---

_@BurntSushi approved on 2024-07-08 15:01_

---

_Comment by @zanieb on 2024-07-08 15:17_

As a side-note, are we tracking not hard-failing on errors during pre-fetch? Seems wild that this can crash the resolution.

---

_Comment by @charliermarsh on 2024-07-08 16:26_

@zanieb - I don't think so... Part of the problem is that we _do_ need to track failures here, we can't just skip silently, since if we later request the package and it fails in a way that we consider a hard failure (like a build failure), that does need to be surfaced.

---

_Comment by @charliermarsh on 2024-07-08 16:26_

I created https://github.com/astral-sh/uv/issues/4901.

---

_Merged by @charliermarsh on 2024-07-08 16:32_

---

_Closed by @charliermarsh on 2024-07-08 16:32_

---

_Branch deleted on 2024-07-08 16:32_

---
