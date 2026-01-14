```yaml
number: 17479
title: Add debounce to pyx token refresh
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: claude/credential-helper-token-refresh-oq5GB
created_at: 2026-01-14T23:39:06Z
updated_at: 2026-01-14T23:39:06Z
url: https://github.com/astral-sh/uv/pull/17479
synced_at: 2026-01-14T23:42:42Z
```

# Add debounce to pyx token refresh

---

_@zanieb_

Prevent concurrent processes from all refreshing the token simultaneously by using a file lock and timestamp-based debounce. When a refresh is needed:

1. Acquire an exclusive lock on refresh.lock
2. Check if another process recently refreshed (within 5 seconds)
3. If so, re-read the tokens from disk instead of making another request
4. Otherwise, perform the refresh and update the timestamp

This prevents thundering herd issues when multiple concurrent processes are invoking `uv auth token` or `uv auth credential-helper` at the same time.

---
