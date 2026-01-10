```yaml
number: 15679
title: Invert the logic for determining if a path is a base conda environment
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/conda-name
created_at: 2025-09-04T13:20:07Z
updated_at: 2025-09-17T11:04:32Z
url: https://github.com/astral-sh/uv/pull/15679
synced_at: 2026-01-10T06:36:15Z
```

# Invert the logic for determining if a path is a base conda environment

---

_Pull request opened by @zanieb on 2025-09-04 13:20_

Closes https://github.com/astral-sh/uv/issues/15604

The previous logic does not match the discussion in the original issue about this feature, nor does it match the comment for the function. I'm confused because I know this logic is working for some people? I'm consequently a little wary of making this change. I'm following up with some additional changes that should ensure this is robust, e.g., #15680 

---

_Marked ready for review by @zanieb on 2025-09-15 20:29_

---

_Label `bug` added by @konstin on 2025-09-16 07:41_

---

_@konstin approved on 2025-09-16 07:43_

---

_Merged by @zanieb on 2025-09-17 11:04_

---

_Closed by @zanieb on 2025-09-17 11:04_

---

_Branch deleted on 2025-09-17 11:04_

---
