```yaml
number: 1071
title: Track HTTP caches for URL wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/http
created_at: 2024-01-23T22:05:57Z
updated_at: 2024-01-23T22:31:43Z
url: https://github.com/astral-sh/uv/pull/1071
synced_at: 2026-01-10T15:39:03Z
```

# Track HTTP caches for URL wheels

---

_Pull request opened by @charliermarsh on 2024-01-23 22:05_

## Summary

This PR ensures that we store HTTP caching information for wheels. Previously, we only stored these for source distributions. This will be helpful for refresh, since we can avoid re-downloading wheels that are unchanged per HTTP caching semantics.

There should be zero performance hit here for warm installs, and only an extremely small hit for cold installs (writing the HTTP cache data to disk). The hyperfine benchmarks reflect this.

---

_Label `enhancement` added by @charliermarsh on 2024-01-23 22:06_

---

_Merged by @charliermarsh on 2024-01-23 22:31_

---

_Closed by @charliermarsh on 2024-01-23 22:31_

---

_Branch deleted on 2024-01-23 22:31_

---
