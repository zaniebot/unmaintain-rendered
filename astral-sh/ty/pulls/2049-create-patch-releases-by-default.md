```yaml
number: 2049
title: Create patch releases by default
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/patch
created_at: 2025-12-18T02:16:31Z
updated_at: 2025-12-18T04:13:54Z
url: https://github.com/astral-sh/ty/pull/2049
synced_at: 2026-01-12T15:54:28Z
```

# Create patch releases by default

---

_@charliermarsh_

## Summary

Removing `default-bump-type = "pre"` will tell Rooster to default to patch releases, now that the Beta is out.


---

_Review requested from @zanieb by @charliermarsh on 2025-12-18 02:16_

---

_@carljm approved on 2025-12-18 02:31_

So "patch" is the default bump type?

---

_Comment by @charliermarsh on 2025-12-18 02:37_

Yeah I believe so ([source](https://github.com/zanieb/rooster/blob/da1ac78d2fe5aa1926fe1a492eb657aa2d8143a7/src/rooster/_config.py#L56)). This matches what we use in `uv`.

---

_Merged by @charliermarsh on 2025-12-18 02:37_

---

_Closed by @charliermarsh on 2025-12-18 02:37_

---

_Branch deleted on 2025-12-18 02:37_

---

_Comment by @zanieb on 2025-12-18 04:13_

Yep, thanks!

---
