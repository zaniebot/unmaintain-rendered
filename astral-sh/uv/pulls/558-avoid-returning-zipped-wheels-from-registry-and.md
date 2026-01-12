```yaml
number: 558
title: Avoid returning zipped wheels from registry and URL indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2023-12-05T01:26:14Z
updated_at: 2023-12-05T08:53:46Z
url: https://github.com/astral-sh/uv/pull/558
synced_at: 2026-01-12T16:04:02Z
```

# Avoid returning zipped wheels from registry and URL indexes

---

_@charliermarsh_

## Summary

This is hard to reproduce, but if you run a long installation process that errors part-way through, you can end up with zipped wheels in the `Wheels` cache, which is intended to contain only unzipped wheels. This PR avoids returning those entries from the registry, which will then lead to errors downstream when we treat them as directories.

---

_Review requested from @konstin by @charliermarsh on 2023-12-05 01:26_

---

_Label `bug` added by @charliermarsh on 2023-12-05 01:26_

---

_@konstin approved on 2023-12-05 08:53_

---

_Merged by @konstin on 2023-12-05 08:53_

---

_Closed by @konstin on 2023-12-05 08:53_

---

_Branch deleted on 2023-12-05 08:53_

---
