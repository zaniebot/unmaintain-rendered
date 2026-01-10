```yaml
number: 1440
title: Decode HTML escapes when extracting SHA
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/esc
created_at: 2024-02-16T06:12:11Z
updated_at: 2024-02-16T06:15:52Z
url: https://github.com/astral-sh/uv/pull/1440
synced_at: 2026-01-10T15:33:24Z
```

# Decode HTML escapes when extracting SHA

---

_Pull request opened by @charliermarsh on 2024-02-16 06:12_

## Summary

If a distribution contains a `+`, it'll be HTML-escaped; so when we try to identify the `#`, we'll split in the wrong location.

Closes https://github.com/astral-sh/uv/issues/1338.


---

_Merged by @charliermarsh on 2024-02-16 06:15_

---

_Closed by @charliermarsh on 2024-02-16 06:15_

---

_Branch deleted on 2024-02-16 06:15_

---
