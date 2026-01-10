```yaml
number: 1000
title: Allow relative paths and environment variables in all editable representations
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/verbatim
created_at: 2024-01-19T04:38:10Z
updated_at: 2024-01-19T14:00:38Z
url: https://github.com/astral-sh/uv/pull/1000
synced_at: 2026-01-10T15:39:03Z
```

# Allow relative paths and environment variables in all editable representations

---

_Pull request opened by @charliermarsh on 2024-01-19 04:38_

## Summary

I don't know if this is actually a good change, but it tries to make the editable install experience more consistent. Specifically, we now support...

```
# Use a relative path with a `file://` prefix.
# Prior to this PR, we supported `file:../foo`, but not `file://../foo`, which felt inconsistent.
-e file://../foo

# Use environment variables with paths, not just URLs.
# Prior to this PR, we supported `file://${PROJECT_ROOT}/../foo`, but not the below.
-e ${PROJECT_ROOT}/../foo
```

Importantly, `-e file://../foo` is actually not supported by pip... `-e file:../foo` _is_ supported though. We support both, as of this PR. Open to feedback.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-19 04:38_

---

_Review requested from @konstin by @charliermarsh on 2024-01-19 04:38_

---

_Label `enhancement` added by @charliermarsh on 2024-01-19 04:38_

---

_@charliermarsh reviewed on 2024-01-19 04:39_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:104 on 2024-01-19 04:39_

This is now just a string.

---

_@charliermarsh reviewed on 2024-01-19 04:39_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:127 on 2024-01-19 04:39_

This is janky.

---

_Converted to draft by @charliermarsh on 2024-01-19 04:41_

---

_Marked ready for review by @charliermarsh on 2024-01-19 04:58_

---

_@konstin approved on 2024-01-19 08:42_

---

_Merged by @charliermarsh on 2024-01-19 14:00_

---

_Closed by @charliermarsh on 2024-01-19 14:00_

---

_Branch deleted on 2024-01-19 14:00_

---
