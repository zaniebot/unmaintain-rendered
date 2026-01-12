```yaml
number: 9785
title: "Don't fail with `--no-build` when static metadata is available"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/opt
created_at: 2024-12-10T19:50:47Z
updated_at: 2024-12-10T20:10:52Z
url: https://github.com/astral-sh/uv/pull/9785
synced_at: 2026-01-12T16:08:58Z
```

# Don't fail with `--no-build` when static metadata is available

---

_@charliermarsh_

## Summary

This optimization isn't quite right, because we can successfully extract metadata without having to build from source. (The builder itself will error if we reach the point at which we need to build, but builds are disabled.)

Closes https://github.com/astral-sh/uv/issues/9776.


---

_Label `bug` added by @charliermarsh on 2024-12-10 19:50_

---

_Review requested from @konstin by @charliermarsh on 2024-12-10 19:50_

---

_Comment by @charliermarsh on 2024-12-10 19:53_

I traced it back, and it looks like this code was added at the point in time when we _always_ ran PEP 517 build hooks. Now, we can't know if we have to run hooks until we've downloaded the distribution -- so it's correct to continue, and fail later.

---

_Merged by @charliermarsh on 2024-12-10 20:10_

---

_Closed by @charliermarsh on 2024-12-10 20:10_

---

_Branch deleted on 2024-12-10 20:10_

---
