```yaml
number: 14346
title: Drop trailing slashes when converting index URL from URL
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slashes
created_at: 2025-06-29T13:18:16Z
updated_at: 2025-06-29T13:36:15Z
url: https://github.com/astral-sh/uv/pull/14346
synced_at: 2026-01-10T06:53:01Z
```

# Drop trailing slashes when converting index URL from URL

---

_Pull request opened by @charliermarsh on 2025-06-29 13:18_

## Summary

In #14245, we started normalizing index URLs by dropping the trailing slash in the lockfile. We added tests to ensure that this didn't cause existing lockfiles to be invalidated, but we missed one of the constructors (specifically, the path that's used with `tool.uv.sources`).


---

_Label `bug` added by @charliermarsh on 2025-06-29 13:18_

---

_Marked ready for review by @charliermarsh on 2025-06-29 13:18_

---

_Merged by @charliermarsh on 2025-06-29 13:36_

---

_Closed by @charliermarsh on 2025-06-29 13:36_

---

_Branch deleted on 2025-06-29 13:36_

---
