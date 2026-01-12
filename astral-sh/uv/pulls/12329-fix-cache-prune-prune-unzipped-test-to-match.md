```yaml
number: 12329
title: "Fix cache_prune::prune_unzipped test to match updated error message format"
type: pull_request
state: closed
author: chhoumann
labels: []
assignees: []
base: main
head: fix-cache-prune-snapshot
created_at: 2025-03-20T07:30:12Z
updated_at: 2025-03-20T07:40:00Z
url: https://github.com/astral-sh/uv/pull/12329
synced_at: 2026-01-12T16:10:13Z
```

# Fix cache_prune::prune_unzipped test to match updated error message format

---

_@chhoumann_

## Summary

Fixes the failing `cache_prune::prune_unzipped` test that was causing CI failures in my other PR (#12328) and others like PR #12327.

The error message format changed to show a specific version constraint (`iniconfig<=2.0.0`) rather than the generic 'all versions' message. This PR updates the test to expect the new, more specific error message.

## Test Plan
Ran `cargo test -p uv cache_prune::prune_unzipped` to verify the test now passes.

---

_Comment by @chhoumann on 2025-03-20 07:39_

Closing in favor of PR #12330 which has a clean branch with only the test fix. The previous PR accidentally included changes from my feature PR #12328.

---

_Closed by @chhoumann on 2025-03-20 07:39_

---

_Branch deleted on 2025-03-20 07:40_

---
