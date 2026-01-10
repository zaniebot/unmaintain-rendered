```yaml
number: 11298
title: Fix marker merging for requirements.txt for psycopg
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-marker-merging
created_at: 2025-02-06T21:21:15Z
updated_at: 2025-02-06T21:31:54Z
url: https://github.com/astral-sh/uv/pull/11298
synced_at: 2026-01-10T11:10:35Z
```

# Fix marker merging for requirements.txt for psycopg

---

_Pull request opened by @konstin on 2025-02-06 21:21_

Given an input in the shape:

```
foo[bar]==1.0.0; sys_platform == 'linux'
foo==1.0.0; sys_platform != 'linux'
```

We would write either

```
foo==1.0.0; sys_platform == 'linux'
```
or
```
foo==1.0.0
```

depending on the iteration order, as the first one is from the marker proxy package and the second one from the package without marker.

The fix correctly merges graph entries when there are two nodes with different extras and different markers.

I tried to write a packse test but it failed due to a different iteration order showing the correct case directly instead of the failing one we'd need.

Only `strip_extras` is affected, since `combine_extras` uses `version_marker`.

---

_Label `bug` added by @konstin on 2025-02-06 21:21_

---

_Review requested from @charliermarsh by @konstin on 2025-02-06 21:21_

---

_@charliermarsh approved on 2025-02-06 21:25_

---

_Merged by @konstin on 2025-02-06 21:31_

---

_Closed by @konstin on 2025-02-06 21:31_

---

_Branch deleted on 2025-02-06 21:31_

---
