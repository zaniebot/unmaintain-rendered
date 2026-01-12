```yaml
number: 10853
title: All (virtual) packages must have a priority
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/priorities-debugging
created_at: 2025-01-22T12:01:05Z
updated_at: 2025-01-23T16:09:48Z
url: https://github.com/astral-sh/uv/pull/10853
synced_at: 2026-01-12T16:09:31Z
```

# All (virtual) packages must have a priority

---

_@konstin_

To ensure deterministic resolution, each (virtual) package needs to be registered on discovery (as dependency of another package), before we query it for prioritization.

Also adds a test for priority value size, prioritization is performance critical in pubgrub.

Not sure what is going with the transformers snapshot.

---

_Label `enhancement` added by @konstin on 2025-01-22 12:01_

---

_Review requested from @charliermarsh by @konstin on 2025-01-22 12:01_

---

_Comment by @charliermarsh on 2025-01-22 17:26_

@konstin -- I made these debug asserts just in case. Is that ok?

---

_Comment by @konstin on 2025-01-23 10:15_

That works too

---

_Merged by @konstin on 2025-01-23 16:09_

---

_Closed by @konstin on 2025-01-23 16:09_

---

_Branch deleted on 2025-01-23 16:09_

---
