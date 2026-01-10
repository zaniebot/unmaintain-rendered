```yaml
number: 12057
title: Insert dependencies into fork state prior to fetching metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - performance
assignees: []
merged: true
base: main
head: charlie/index
created_at: 2025-03-07T19:28:20Z
updated_at: 2025-03-07T19:45:48Z
url: https://github.com/astral-sh/uv/pull/12057
synced_at: 2026-01-10T11:10:39Z
```

# Insert dependencies into fork state prior to fetching metadata

---

_Pull request opened by @charliermarsh on 2025-03-07 19:28_

## Summary

The order here is slightly off... As-is, we fetch the metadata for the dependency, _then_ insert the URLs and indexes into the fork state -- so the fetch doesn't take the explicit index or URL into account. This has mostly been unobserved because we re-fetch anyway in the next request, but if we do things in the right order (add to fork state, fetch dependencies, insert dependencies), we can cut down on the fetches.

Closes https://github.com/astral-sh/uv/issues/12056.


---

_Label `bug` added by @charliermarsh on 2025-03-07 19:28_

---

_Label `performance` added by @charliermarsh on 2025-03-07 19:28_

---

_Merged by @charliermarsh on 2025-03-07 19:45_

---

_Closed by @charliermarsh on 2025-03-07 19:45_

---

_Branch deleted on 2025-03-07 19:45_

---
